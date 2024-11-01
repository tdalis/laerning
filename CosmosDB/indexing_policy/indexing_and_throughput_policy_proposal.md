# Indexing and Throughput Policy Proposal for DTDI Azure Cosmos DB Container

This document aims to help us decide our throughput and indexing policies for containers.

## Throughput

Containers are the fundamental unit of scalability in Azure Cosmos DB for NoSQL. Typically, you provision throughput at the container level but since we have chosen provisional throughput on the account level, the container is limited to the provisioned throughput. You can provision throughput on the database level as well, but depending on the use-case you might want to provision the throughput on the container level.

If we decide to provision throughput on database or container level, we have 2 options:
1. Autoscale
2. Manual

If a container is expected to receive more or larger items (JSON files), then we might want to provision manual throughput and allocate more throughput for the container that is expected to receive more or larger items. But this is too simplistic of an approach.

Since the database throughput is shared by the containers, then, naturally, the account throughput is also shared by the containers. If the strategy is to provision manual throughput because we know that the workloads will have a sustained traffic requiring predictable performance and to control cost optimisation then we might be tempted to continue with the same strategy and use manual throughput for each container based on their predicted performance needs.

[Throughput Comparison](https://learn.microsoft.com/en-us/azure/cosmos-db/throughput-serverless)

It is important to keep in mind that the containers evenly distribute the provisioned throughput among their physical partitions. If we have two containers, container A and container B, the partition key it is decided to be the ```vehicleId``` as it is defined in the LLD. We also know from the LLD, that APC receives much higher volumes of data per day (see table taken from LLD below). With this in mind, although the TCMS files are larger in size than the APC files, the APC files volume is much larger which means that the throughput is important for both, APC requires RU/s for processing a larger number of physical partitions and TCMS needs RU/s to process larger files. Therefore, we propose to use autoscale throughput both containers.


During testing, to confirm that this is the right strategy, we can analyse the <strong>Normalised RU Consumption</strong> metric for APC and TCMS containers. If the average normalised utilisation is less than 66%, we can keep autoscale, if it's over 66% then it is recommended that we remain on standard (manual) provisioned throughput.

<em>The average normalised utilisation is calculated by averaging the highest normalised utilisation for each hour across all hours. [Read this resource from Microsoft for more details](https://learn.microsoft.com/en-us/azure/cosmos-db/how-to-choose-offer)</em>

## Indexing

In Azure Cosmos DB, every container has an indexing policy that dictates how the container's items should be indexed. The default indexing policy for newly created containers indexes every property (key-value pair) of every item and enforces range indexes for any string or number. This allows you to get good query performance without having to think about indexing and index management upfront.

In some situations, we might want to override this automatic behaviour.

There is a number of [different index types](https://learn.microsoft.com/en-us/azure/cosmos-db/index-overview) available with Cosmos DB for NoSQL containers, with the default being the range type as mentioned above. For APC and TMCS data, we want to consider the range and composite index types.

### Range index

<strong>Range</strong> indexes are based on an ordered tree-like structure. The range index type is for:

- Equality queries:
```SQL
SELECT * FROM container c WHERE c.property = 'value'
```

```SQL
SELECT * FROM c WHERE c.property IN ("value1", "value2", "value3")
```

- Equality match on an array element
```SQL
SELECT * FROM c WHERE ARRAY_CONTAINS(c.tags, "tag1")
```

- Range queries:
```SQL
SELECT * FROM container c WHERE c.property > 'value'
```

```Also works with >, <, >=, <=, !=```

- Checking for the presence of a property:
```SQL
SELECT * FROM c WHERE IS_DEFINED(c.property)
```

- String system functions:
```SQL
SELECT * FROM c WHERE CONTAINS(c.property, "value")
```

```SQL
SELECT * FROM c WHERE STRINGEQUALS(c.property. "value")
```

- ```ORDER BY``` queries:
```SQL
SELECT * FROM container c ORDER BY c.property
```

- ```JOIN``` queries:
```SQL
SELECT child FROM container c JOIN child IN c.properties WHERE child = 'value'
```

Range indexes can be used on scalar values (strong or number). As a reminder, this is the default indexing policy for newly cerated containers.

### Composite index

The ```ORDER BY``` clause that orders a single property <em><strong>always</strong></em> needs a range index and it will fail if the path it references doesn't have one. The issue with the default range index is that it <strong>does not</strong> support ```ORDER BY``` queries which order by <em>multiple</em> properties. To order by multiple properties we always need to have a composite index.

The good news is that we can add the composite index in our default policy so that the range and composite index types work together. In fact, it seems like all types of indexes can work in parallel but if we want to use them we need to define them.

Defining composite indexing takes time and effort because we need to be explicit with our definitions of paths and ascending or descending order of the value within the indexed path. If we expect to have multiple combinations of composite index paths and ascending orders we will have to define multiple properties.

It's easier to understand this with examples.

Let's say we want to define a composite index for ```name asc, age desc```, we will define it like in the below JSON file:

```JSON
{  
    "automatic":true,
    "indexingMode":"Consistent",
    "includedPaths":[  
        {  
            "path":"/*"
        }
    ],
    "excludedPaths":[],
    "compositeIndexes":[  
        [  
            {  
                "path":"/name",
                "order":"ascending"
            },
            {  
                "path":"/age",
                "order":"descending"
            }
        ]
    ]
}
```

The following considerations are used when using composite indexes for queries with an ```ORDER BY``` clause with two or more properties:

- If the composite index paths don't match the sequence of the properties in the ```ORDER BY``` clause, then the composite index can't support the query.
    - This means that we cannot use the below query with the above example:
    ```SQL
    SELECT * FROM c ORDER BY c.age DESC, c.name ASC
    ```
    - It will work with the following queries:
    Query #1:
    ```SQL
    SELECT * FROM c ORDER BY c.name ASC, c.age DESC
    ```
    Query #2
    ```SQL
    SELECT * FROM c ORDER BY c.name DESC, c.age ASC
    ```
    It also optimises the filters such as:
    Query #3:
    ```SQL
    SELECT * FROM c WHERE c.name = "Tim" ORDER BY c.name DESC, c.age ASC
    ```
    Query #4:
    ```SQL
    SELECT * FROM c WHERE c.name = "Tim" AND c.age > 18
    ```
    Again, notice that the order in which we use name and age is important.
- The order of composite index paths (ascending or descending) should also match the order in the ```ORDER BY``` clause (as explained with the examples above)
- The composite index also supports an ```ORDER BY``` clause with the opposite order on all paths (again, as we've seen with the examples above).

Some more good examples taken from [Microsoft's learn portal](https://learn.microsoft.com/en-us/azure/cosmos-db/index-policy#:~:text=ORDER%20BY%20queries%20on%20multiple%20properties%3A):

| <strong>Composite Index</strong> | <strong>Sample ```ORDER BY``` Query</strong> | <strong>Supported by Composite Index?</strong> |
|----------------------------------|----------------------------------------------|-----------------|
| ```(name ASC, age DESC)``` | ```SELECT * FROM c ORDER BY c.name ASC, c.age asc``` | ```YES``` |
| ```(name ASC, age ASC)``` | ```SELECT * FROM c ORDER BY c.age ASC, c.name asc``` | ```No``` |
| ```(name ASC, age ASC)``` | ```SELECT * FROM c ORDER BY c.name DESC, c.age DESC``` | ```Yes``` |
| ```(name ASC, age ASC)``` | ```SELECT * FROM c ORDER BY c.name ASC, c.age DESC``` | ```No``` |
| ```(name ASC, age ASC, timestamp ASC)``` | ```SELECT * FROM c ORDER BY c.name ASC, c.age ASC, timestamp ASC``` | ```Yes``` |
| ```(name ASC, age ASC, timestamp ASC)``` | ```SELECT * FROM c ORDER BY c.name ASC, c.age ASC``` | ```No``` |

The last example showcases that if you define a property in the composite index but it is not part of the query, the query will fail.

We should customise our indexing policy so we can serve all necessary   ```ORDER BY``` queries. The composite indexing is also useful when a query has filters on two or more properties because it makes the query more efficient, taking less time and consuming fewer RUs.

Finally, reading [this post in Microsoft learn](https://learn.microsoft.com/en-us/azure/cosmos-db/index-overview#:~:text=indexing%20policy%20examples-,Composite%20indexes,-Composite%20indexes%20increase), we know that <strong>composite</strong> indexes increase the efficiency when we're performing operations on multiple fields. The composite index type is used for:

- ```ORDER BY``` queries on multiple properties:
```SQL
SELECT * FROM container c ORDER BY c.property1, c.property2
```
- Queries with a filter and ```ORDER BY```. These queries can utilise a composite index if the filter property is added to the ```ORDER BY``` clause:
```SQL
SELECT * FROM container c WHERE c.property1 = 'value' ORDER BY c.property1, c.property2
```
- Queries with a filter on two or more properties where at least one property is an equality filter:
```SQL
SELECT * FROM container c WHERE c.property1 = 'value' AND c.property2 > 'value'
```

To assess whether we need composite index, we have to define the queries we expect our API to use and if there are queries that have:
- ```ORDER BY``` on multiple properties
- A filter and ```ORDER BY``` so that they can utilise a composite index if the filter property is added to the ```ORDER BY``` clause
- A filter on two or more properties where at least one property is an equality filter.

The definition of the queries will be derived from the business needs. For example, one of my recent projects required me to build GET API end-points for specific IoT business usages. Understanding who in the business would use the API end-points and what answers they are looking to get from the data allows us to define our queries.

Once we have defined our queries, we can then assess whether they meet the criteria for composite index requirements.