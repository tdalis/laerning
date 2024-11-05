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

## MLOps Stack

This section explores the technologies and platforms for achieving our MLOps goals.

## MLOps Stack requirements

First, we need to understand what features and requirements the tools we choose must provide and meet.

The whole MLOps lifecycle, can be broadly categorised in three stages:

- Data gathering
- Modeling
- Deployment

This means, that three technology worlds need to come together in one place. With each stage being comprised of multiple steps and tasks, different tools will be used at each stage.

<strong>Data gathering & preparation</strong>

For the data gathering & preparation we need tools that can help us:

- Gather data
- Clean data
- Transform batch and streaming data pipelines

<strong>Source Control</strong>

we also need a ```Source Control``` tool for versioning the code, the data, the data and model artifacts, and more. The MLOps sees reproducibility as a key challenge to solve for ML projects and a good source control strategy and tool will help with that.

<strong>Research and Experimentation</strong>

Another key activity is ```Research and Experimentation```. We need a platform that's: 
- Data scientist friendly - this means it should work out of the box with popular ML frameworks so that innovation is not restricted. At the very least, the platform should support popular frameworks such as:
    - PyTorch
    - Keras
    - TensorFlow
    - ScikitLearn
    - Catalyst
- Host notebooks for development that can enable data scientists to perform rapid experimentation while keeping track of all the result metrics of the experiments.

<strong>Hyperparameter tuning</strong>
We also need a platform that enables data scientists to do ```Hyperparameter tuning```. Choosing the correct hyperparameters for machine learning models is very useful to help improve the model's performance. Hence, we need a hyperparameter optimisation framework to help data scientist select the best combination of hyperparameters that deliver the best model performance.

<strong>Distributed model training</strong>

Our MLOps platform should be able to automatically train the model on different sets of data. We know that model training is resource expensive and it may require GPUs for days or weeks. To address this issue we need a platform that offers ```Auto-scaling``` and ```distributed model training```. Some MLOps platforms come with pre-made pipelines but not all MLOps platforms come with that. At the very least we should expect: 

- ```A selection of reusable components``` & ```Automation triggers``` for assembling custom training pipelines.
- ```CI/CD tools``` for scheduling, task queuing, and smart alerts.
- Integration with popular container services and orchestration of containerised batch jobs (i.e. ```Docker``` & ```Kubernetes``` support).

<strong>Auto ML</strong>

Automated model training can save time when trying to discover the best model and feature fit for our problem and data.

<strong>Deployment</strong>

Deployment is a complex task. It requires a lot of activities to get done. For example:

- Collection of the various model artifacts
- Setting up a service stack
- Exposing an endpoint
- Emitting real-time metrics
- And more

To help with all these tasks, we are looking for a platform that offers ```Automated deployment``` which uses container services such as Docker and Kubernetes to allow for model autoscaling.

Deployment is done on some model servers that allow you to serve any model you train as real-time HTTPS endpoints. The things to keep in mind for model servers:

- Fallback support - this will give you the ability to switch models when your new model is problematic until you have fixed whatever issues have occurred.
- Easy scale out
- Canary A/B framework - to allow us to roll out the deployment to a small subset of users.
- Co-locate multiple models - this is for scenarios where we have multiple models where the output of one model is the input of another. A model server that allows us to co-locate multiple models on the same server or at-least provide an easy connection between the models will be helpful in such scenario.
- Security

<strong>Model registry service</strong>

A model registry is a centralised repo for hosting all the important artifacts for production ready models. Some MLOps platforms provide ready to use model registries, others provide us with a tool-kit for assembling one. At the very minimum, a good model registry needs to include:

- Training data
- Model type
- Artifacts
- Training parameters
- Hyperparameters
- Performance metrics accuracy
- Precision
- Any extra data that we need to effectively deploy the model on run-time.

<strong>Feature vector storage</strong>

There is hardly any data that can work with models in their raw format. Therefore, feature engineering is a key component of a machine learning project. A feature vector storage allows the MLOps teams and projects to share and reuse the features they engineer.

Having a feature store in place helps because:

- It facilitates collaboration between teams
- It reduces duplication by reusing features
- It reduces the feature engineer costs per project since data scientists and engineers won't need to re-engineer the same data as well as there won't be extra memory usage from having duplicated features

<strong>Monitoring and observability services</strong>

Monitoring is key to help us know when things go wrong before our users and customers do so we can fix it in a timely manner. A good monitoring tool should:

- Give real-time performance view of the deployed models
- Have alert mechanisms which can alert a person or trigger a retraining of the model upon the detection of data drift or deviation in model performance

<strong>Governance</strong>

It's beneficial if the platform we use provides model governance services to control access, implement policy and track model activity.

The platform should also give the capability to track end-to-end trail of ML assets by using the metadata in the ML metadata store. Using the ML metadata store should allow you to explain your models, help you meet the regulatory compliance, and understand how models arrive at a result for a given input.

<strong>Compliance and Audit Services</strong>

In most regulated industries, such as central government and energy, there can be an additional compliance and audit stage where models are reviewed by internal or external auditors. This is mandatory in models where risk is involved, such as models that manage financial outcomes since such models will have direct impact on the finances of a business or a customer, making it extremely important to detect and correct any biased or unethical outcomes. therefore, auditing the model inferences with a service will be a great add-on.

ML tools for compliance and audit often work with the model governance for authentication and authorisation for things like: 

- What our members have access to what models
- Who can deploy a model to what environments
- Who can approve the production movement
- More...

Below we are going to explore some tools that offer those services. In many cases one tool or service can offer multiple of the services we explored above.

## MLOps platforms comparison

It might be a bit too early to expect to have a mature end-to-end MLOps platform in place. However, the three key cloud providers, GCP, Azure, AWS, provide a lot of tools to help meet the majority of the MLOps needs.

Besides the cloud providers, a new breed of players started to leverage the cloud providers and integrate their open source tools to build their own MLOps platforms. Each of these players has reached different levels of development.

Neither the cloud platforms or the other smaller players are fully matured in regards to the MLOps solutions they provide.

The Udemy course, compares tools for four key stages of MLOps, with the caveat that these tools are not by any means the best tools. We can use this list for our research of tools when we need to though. Maturity level is represented in colours, the darker the colour the more mature the tool in the category:

| <strong>Tool Name</strong> | <strong>Data gathering & transformation</strong> | <strong>Experimentation, training, testing, tuning</strong> | <strong>Deployment & Inference</strong> | <strong>Monitoring, Auditing, Management, Retraining</strong> |
-----------------------------|--------------------------------------------------|------------------------------------------------------------|----------------------------------------|--------------------------------|
| Pachyderm | ![mature](imgs/maturity_colours_dark_blue.png) | ![not very mature](imgs/maturity_colours_light-blue.png) | ![mature](imgs/maturity_colours_dark_blue.png) | ![not at all](imgs/maturity_colours_white.png) |
| Algorithmia | ![not at all](imgs/maturity_colours_white.png) | ![not at all](imgs/maturity_colours_white.png) | ![mature](imgs/maturity_colours_dark_blue.png) | ![mature](imgs/maturity_colours_dark_blue.png) |
| MLflow | ![not at all](imgs/maturity_colours_white.png) | ![very small maturity](imgs/maturity_colours_light-grey.png) | ![very small maturity](imgs/maturity_colours_light-grey.png) | ![very small maturity](imgs/maturity_colours_light-grey.png) |
| Kubeflow | ![very small maturity](imgs/maturity_colours_light-grey.png) | ![very small maturity](imgs/maturity_colours_light-grey.png) | ![very small maturity](imgs/maturity_colours_light-grey.png) | ![not at all](imgs/maturity_colours_white.png) |
| Polyaxon | ![not at all](imgs/maturity_colours_white.png) | ![not very mature](imgs/maturity_colours_light-blue.png) | ![mature](imgs/maturity_colours_dark_blue.png) | ![not very mature](imgs/maturity_colours_light-blue.png) |
| Valohai | ![not very mature](imgs/maturity_colours_light-blue.png) | ![not very mature](imgs/maturity_colours_light-blue.png) | ![not very mature](imgs/maturity_colours_light-blue.png) | ![not very mature](imgs/maturity_colours_light-blue.png) |
| Allegro | ![somewhat mature](imgs/maturity_colours_dark-grey.png) | ![somewhat mature](imgs/maturity_colours_dark-grey.png) | ![very small maturity](imgs/maturity_colours_light-grey.png) | ![somewhat mature](imgs/maturity_colours_dark-grey.png) |

Colour coding:

- Mature = ![mature](imgs/maturity_colours_dark_blue.png)
- Somewhat Mature = ![somewhat mature](imgs/maturity_colours_dark-grey.png)
- Very small maturity = ![very small maturity](imgs/maturity_colours_light-grey.png)
- Not very mature = ![not very mature](imgs/maturity_colours_light-blue.png)
- Doesn't cater to feature = ![not at all](imgs/maturity_colours_white.png)

This was based on the time the tutor recorded the Udemy course. It might be different today and we might need to do our own due-diligence when we need to choose tools.

## Which platform to choose?

To help us choose, we need to ask some questions first. The first question is probably whether we want to use a cloud or not cloud environment. Let's check the table below for example scenarios.


| <strong>Question</strong> | <strong>Option 1</strong> | <strong>Option 2</strong> |
---------------------------|---------------------------|--------------------------|
| Do you want managed or unmanaged solutions? | <strong>Managed</strong>: i.e., Algorithmia, SageMaker, Google ML, Paperspace | <strong>Unmanaged</strong>: i.e., Kubeflow, Seldon, TensorFlow |
| What are the data security requirements of your organisation? | <strong>Critical data</strongs>: i.e., don't want the data in the cloud, then Algorithmia, Seldon, TensorFlow, Kubeflow | <strong>Non critical data</strong>: Cloud MLOps solution. |
| What is the complexity of your models? Would GPU inferences be needed? If yes, probably cloud will be your best option. | <strong>Cloud solutions</strong>: i.e., AWS, Google, Azure cloud | <strong>On-premise</strong>: i.e., Algorithmia |
| Containerisation is must | <strong>Custom container</strong>: Kubeflow, Seldon AI | <stroung>Cloud provider</strong>: Algorithmia, SageMaker, Google AI |
| CLI vs GUI? This would depend on the team's preferences and needs | <strong>CLI</strong>: i.e., Seldon, Cortex, Optuma | <strong>GUI</strong>: i.e., Kubeflow, MLflow, Polyaxon |
| Which programing languages and libraries your team is comfortable in? Most languages are supported by most platforms so this question might be best answered by looking what is not supported by what platform. | <strong>No Java support</strong>: Polyaxon, Comet, Optuma | <strong>No ONNX support</strong>: i.e, Kubeflow, Polyaxon |
| What type of machine learning objectives do you want to achieve? | <strong>Traditional ML</strong>: i.e., MLflow, MetaFlow | <strong>Deep learning</strong>: i.e., Valohai, Allegro (better for GPU utilisation) |
| Specialised platform or end-to-end solution? | <strong>Specialised</strong>: i.e., Algorithmia specialising in deployment and monitoring | <strong>End-to-end</strong>: i.e., Valohai offers solutions in all 4 areas (see color-coded table from earlier). |

<em>Again, the comparisons above are not exhaustive and they are based at the time of recording from the tutor.</em>

