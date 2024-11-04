# MLOps Maturity Levels & Tools - Learn MLOps basics of Continuous Integration, Delivery using Azure DevOps and Azure ML. Create MLOps pipeline in Azure

## Three common levels of MLOps

The flowchart we explored in the previous document (ml_ops_basics_solution_proposal) can be a very complex process. We might want to start a bit smaller. We are going to explore 3 maturity levels to see how small we can start and how big we can get.

There are three common MLOps level, starting with level 0 which includes no automation, up to level 2 which automates everything ML and CI/CD pipelines.

We can define the amount of automation based on our project's need.

Google has defined three levels of automation while Microsoft has defined 5.

In the Udemy module, the lecturer is defining 3 maturity levels and he will be categorising each of the maturity based on 8 parameters:

1. People involved in it
2. Data gathering strategy
3. Development activities
4. Model release
5. CI
6. CD
7. App Integration & Testing
8. Monitoring

## MLOps Level 0

MLOps level 0 is the lower maturity level. It's also referred to as no MLOps at all since there are no automations developed at this level.

| <strong>Parameter</strong> | <strong>Implementation</strong> |
|----------------------------|---------------------------------|
| People | Data scientists: siloed, not in regular communications with the larger team |
| Data gathering | Data is scattered everywhere and it is gathered manually |
| Development | All development activities are manual and thrown over the wall to operationalise |
| Model release | Model release is manual and done by data scientists or engineers alone |
| CI | No CI |
| CD | No CD |
| App Integration & Testing | Heavily reliant on Data scientists expertise to implement |
| Monitoring | Lacks activity monitoring |

## MLOps Level 1

The goal in MLOps level 1 is to perform continuous training of the model by automating ML pipeline. This helps to achieve continuous delivery of the model prediction service but not the whole ML Application which acts upon the model inferences.

To help us understand it, silos are broken, helping data scientists and data engineers to work together to convert experimentation code into reputable scripts or jobs. Vice-versa, data engineers also work with data scientists and software engineers to manage inputs and outputs. Finally, software engineers work with data engineers to automate model integration into application form.

Let's make a table like above to represent MLOps Level 1 over the 8 categories.

| <strong>Parameter</strong> | <strong>Implementation</strong> |
|----------------------------|---------------------------------|
| People | Data scientists work directly with engineers to convert experiment to repeatable scripts or jobs |
| Data gathering | Data into databases. Pipelines are created for data gathering, data analysis and data preprocessing. |
| Development | The steps of the ML experiment are orchestrated. Modularised and reusable code is prepared. |
| Model release | Continuous delivery of model |
| CI | No CI Pipeline |
| CD | No Continuous deployment. A continuous training pipeline is deployed into production using a semi manual deployment strategy |
| App Integration & Testing | Unit and integration tests for each model release. Less reliant on data scientist's expertise |
| Monitoring | Active monitoring and retraining triggers are put in place |

*Development, mentions that the code is now modularised. This means that the code is not in a single notebook. A single notebook is not reusable but to construct ML pipelines components need to be reusable, composable, and potentially shareable across ML pipelines. Therefore, in MLOps level 1 the source code for those components is modularised.

## MLOps Level 2

The supreme level of automation.

MLOps two includes all the components of MLOps 1 with a bit extra. It's the highest form of automation achieved in MLOps.

This implementation is so powerful that just by doing a commit in the code, can trigger a set of numerous automated activities and let your pipeline deploy in production. Let's break it down in the table below:

| <strong>Parameter</strong> | <strong>Implementation</strong> |
|----------------------------|---------------------------------|
| People | Data scientists: siloed, not in regular communications with the larger team since they are focusing only on the experimental face where they deploy the packaged source code.|
| Data gathering | Data into databases. Pipelines are created for data gathering, data analysis and data preprocessing. <strong>String data governance, secure APIs, role based data access (least privileged)</strong> |
| Development | The steps of the ML experiment are orchestrated. Modularised and reusable code is prepared. |
| Model release | Continuous delivery of model |
| CI | <strong>Continuous Integration for test, build and package</strong> |
| CD | Continuous delivery, <strong>deployment</strong> and training of models into production |
| App Integration & Testing | <strong>Application code itself contains</strong> the Unit and integration tests for each model release. Less reliant on data scientist's expertise |
| Monitoring | Active monitoring and retraining triggers are put in place |

## Why maturity levels are important

Why not start building the level 2 from the start? Ideally, we want to reach the level 2 MLOps but we can't start from there. Instead, we can use the levels as a set of milestones so that we can work towards level 2 step by step by:

- Estimating the scope of work
- Establish realistic success criteria
- Identify deliverables.

