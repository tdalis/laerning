# Schema and Data Type Mapping in Copy Activity

We are following [this learning path](https://learn.microsoft.com/en-us/azure/data-factory/copy-activity-schema-and-type-mapping#schema-mapping) for this document. The motivation behind this is to help us understand how we can define the schema in Cosmos DB.

## Schema mapping

### Default mapping

By default, copy  activity maps source data to sink* by column names in case-sensitive manner. If sink doesn't exist, for example, writing to file(s), the source field names will be persisted as sink names. If the sink already exists, it must contain all columns being copied from the source. Such default mapping supports flexible schemas and schema drift from source to sink from execution to execution - all the data returned by source data can be copied to sink.

*In Azure Data Factory and Synapse, a sink is a destination data store where data is written to after it has been transformed or copied.

### Explicit Mapping

We can also specify explicit mapping to customise the column/field mapping from source to sink based on our need. With explicit mapping, we can copy only partial source data to sink, or map source data to sink with different names, or reshape tabular/hierarchical data. Copy activity:

1. Reads the data from source and determine the source schema.
2. Applies your defined mapping.
3. Writes the data to sink.

