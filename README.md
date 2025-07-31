---
page_type: sample
languages:
- python
products:
- azure
- azure-machine-learning-service
- azure-devops
description: "MLOps end to end examples & solutions. A collection of examples showing different end to end scenarios operationalizing ML workflows with Azure Machine Learning, integrated with GitHub and other Azure services such as Data Factory and DevOps."
---

# MLOps Guidance on Azure

![](https://i.imgur.com/waxVImv.png)
### [View all Roadmaps](https://github.com/nholuongut/all-roadmaps) &nbsp;&middot;&nbsp; [Best Practices](https://github.com/nholuongut/all-roadmaps/blob/main/public/best-practices/) &nbsp;&middot;&nbsp; [Questions](https://www.linkedin.com/in/nholuong/)
<br/>

To learn the more about the latest guidance from Microsoft about MLOps review the following links.

- [Azure MLOps (v2) Solution Accelerator](https://github.com/Azure/mlops-v2)
- [Set up MLOps with Azure DevOps](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-setup-mlops-azureml?view=azureml-api-2&tabs=azure-shell)
- [Set up MLOps with Github](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-setup-mlops-github-azure-ml?view=azureml-api-2&tabs=azure-shell)
- [Offical Microsoft Documentatation on MLOps](https://learn.microsoft.com/en-us/azure/machine-learning/concept-model-management-and-deployment?view=azureml-api-2)
- [Microsoft Learn cournse for intro to MLOps](https://learn.microsoft.com/en-us/training/paths/introduction-machine-learn-operations/)
- [Microsoft Learn course for E2E MLOps](https://learn.microsoft.com/en-us/training/paths/build-first-machine-operations-workflow/)



------------------------------

# MLOps on Azure
- [![Build Status](https://dev.azure.com/aidemos/MLOps/_apis/build/status/microsoft.MLOps?branchName=master)](https://dev.azure.com/aidemos/MLOps/_build/latest?definitionId=96?branchName=master)
- [Example MLOps Release Pipeline](https://dev.azure.com/customai/DevopsForAI-AML/_release?view=all&_a=releases&definitionId=16)
- [Official Python Azure MLOps repo](https://github.com/nholuongut/MLOps-Guidance-on-AzurePython)
- [MLOps Architecture Deep Dive video](https://www.youtube.com/watch?v=nst3UAGpiBA)

## What is MLOps?
MLOps empowers data scientists and app developers to help bring ML models to production. 
MLOps enables you to track / version / audit / certify / re-use every asset in your ML lifecycle and provides orchestration services to streamline managing this lifecycle.

### MLOps podcast
Check out the recent TwiML podcast on MLOps [here](https://twimlai.com/twiml-talk-321-enterprise-readiness-mlops-and-lifecycle-management-with-jordan-edwards/) 

## How does Azure ML help with MLOps?
Azure ML contains a number of asset management and orchestration services to help you manage the lifecycle of your model training & deployment workflows.

With Azure ML + Azure DevOps you can effectively and cohesively manage your datasets, experiments, models, and ML-infused applications.
![ML lifecycle](./media/ml-lifecycle.png)

## New MLOps features
- [Azure DevOps Machine Learning extension](https://marketplace.visualstudio.com/items?itemName=ms-air-aiagility.vss-services-azureml) 
- [Azure ML CLI](https://aka.ms/azmlcli)
- [Create event driven workflows](https://docs.microsoft.com/azure/machine-learning/service/how-to-use-event-grid) using Azure Machine Learning and Azure Event Grid for scenarios such as triggering retraining pipelines
- [Set up model training & deployment with Azure DevOps](https://docs.microsoft.com/en-us/azure/devops/pipelines/targets/azure-machine-learning?view=azure-devops)

> If you are using the Machine Learning DevOps extension, you can access model name and version info using these variables:
> - Model Name: Release.Artifacts.{alias}.DefinitionName containing model name
> - Model Version: Release.Artifacts.{alias}.BuildNumber 
> where alias is source alias set while adding the release artifact. 

## Getting Started / MLOps Workflow
An example repo which exercises our recommended flow can be found [here](https://github.com/nholuongut/MLOps-Guidance-on-AzurePython)

## MLOps Best Practices
### Train Model
- Data scientists work in topic branches off of master.
- When code is pushed to the Git repo, trigger a CI (continuous integration) pipeline.
- First run: Provision infra-as-code (ML workspace, compute targets, datastores).
- For new code: Every time new code is committed to the repo, run unit tests, data quality checks, train model.

We recommend the following steps in your CI process:
- **Train Model** - run training code / algo & output a [model](https://docs.microsoft.com/en-us/azure/machine-learning/concept-azure-machine-learning-architecture#model) file which is stored in the [run history](https://docs.microsoft.com/en-us/azure/machine-learning/service/concept-azure-machine-learning-architecture#run).
- **Evaluate Model** - compare the performance of newly trained model with the model in production. If the new model performs better than the production model, the following steps are executed. If not, they will be skipped.
- **Register Model** - take the best model and register it with the [Azure ML Model registry](https://docs.microsoft.com/en-us/azure/machine-learning/service/concept-azure-machine-learning-architecture#model-registry). This allows us to version control it.

### Operationalize Model
- You can package and validate your ML model using the **Azure ML CLI**.
- Once you have registered your ML model, you can use Azure ML + Azure DevOps to deploy it.
- You can define a **release definition** in Azure Pipelines to help coordinate a release. Using the DevOps extension for Machine Learning, you can include artifacts from Azure ML, Azure Repos, and GitHub as part of your Release Pipeline.
- In your release definition, you can leverage the Azure ML CLI's **model deploy** command to deploy your Azure ML model to the cloud (ACI or AKS).
- Define your deployment as a [gated release](https://docs.microsoft.com/en-us/azure/devops/pipelines/release/approvals/gates?view=azure-devops). This means that once the model web service deployment in the Staging/QA environment is successful, a notification is sent to approvers to manually review and approve the release. Once the release is approved, the model scoring web service is deployed to [Azure Kubernetes Service(AKS)](https://docs.microsoft.com/en-us/azure/aks/intro-kubernetes) and the deployment is tested.

# MLOps Solutions
We are committed to providing a collection of best-in-class solutions for MLOps, both in terms of well documented & fully managed cloud solutions, as well as reusable recipes which can help your organization to bootstrap its MLOps muscle. These examples are community supported and are not guaranteed to be up-to-date as new features enter the product.

## How is MLOps different from DevOps?
- Data/model versioning != code versioning - how to version data sets as the schema and origin data change
- Digital audit trail requirements change when dealing with code + (potentially customer) data
- Model reuse is different than software reuse, as models must be tuned based on input data / scenario.
- To reuse a model you may need to fine-tune / transfer learn on it (meaning you need the training pipeline)
- Models tend to decay over time & you need the ability to retrain them on demand to ensure they remain useful in a production context.

# What are the key challenges we wish to solve with MLOps?

**Model reproducibility & versioning**
- Track, snapshot & manage assets used to create the model
- Enable collaboration and sharing of ML pipelines

**Model auditability & explainability**
- Maintain asset integrity & persist access control logs
- Certify model behavior meets regulatory & adversarial standards

**Model packaging & validation**
- Support model portability across a variety of platforms
- Certify model performance meets functional and latency requirements

**Model deployment & monitoring**
- Release models with confidence
- Monitor & know when to retrain by analyzing signals such as data drift

## 🚀 I'm are always open to your feedback🇻🇳🇻🇳🇻🇳🇻🇳🇻🇳🇻🇳🇻🇳🇻🇳🇻🇳🇻🇳🇻🇳🇻🇳🇻🇳🇻🇳🇻🇳🇻🇳
![](https://i.imgur.com/waxVImv.png)
# **[Contact Me🇻🇳🇻🇳🇻🇳🇻🇳🇻🇳🇻🇳🇻🇳]**
* [Name: Nho Luong]
* [Skype](luongutnho_skype)
* [Github](https://github.com/nholuongut/)
* [Linkedin](https://www.linkedin.com/in/nholuong/)
* [Email Address](luongutnho@hotmail.com)
* [PayPal.Me](https://www.paypal.com/paypalme/nholuongut)

![](Donate.jpg)
[![ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/nholuong)

![](https://i.imgur.com/waxVImv.png)
# License🇻🇳🇻🇳🇻🇳🇻🇳🇻🇳🇻🇳🇻🇳🇻🇳
* Nho Luong (c). All Rights Reserved.🌟
