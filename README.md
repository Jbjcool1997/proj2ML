Operationalizing Machine Learning with Azure
======
By Jacob Jensen, TDC Net

# Overview

This project showcases how Azure Machine Learning can be used to build and deploy a full machine learning pipeline—from training models to serving predictions through an API in a production environment, complete with monitoring.

The dataset comes from a bank marketing campaign and contains information about individuals contacted via phone. The goal is to predict whether a person is likely to respond positively ("yes") and become a lead.

Dataset link: https://automlsamplenotebookdata.blob.core.windows.net/automl-sample-notebook-data/bankmarketing_train.csv

We begin by using the Azure ML Studio interface to register the dataset and set up an Automated ML experiment to train classification models.

Once the training is complete, we deploy the best-performing model as a REST API web service. Application Insights is enabled to capture telemetry data, and the API comes with a Swagger definition for documentation. The endpoint can be accessed using any HTTP client—for example, a Python script.

Next, we replicate the workflow programmatically using the Azure ML Python SDK in a Jupyter Notebook. This approach allows for greater automation, such as retraining the model automatically when the dataset or code changes. The pipeline integrates smoothly with CI/CD tools like Azure DevOps, enabling automated model training and deployment. It also offers the flexibility to deploy updated models when performance improves.

Finally, we publish the pipeline as a REST API endpoint, which can be triggered to start new experiments. A demonstration of this trigger is provided in the Jupyter Notebook.


## Architectural Diagram

This is an architecture overview of the workflow and the scope of this project.

![image](https://github.com/user-attachments/assets/f4fc06ed-421b-41de-a73b-1792a1261303)


This project involves preparing a dataset for use with Azure's Automated ML to identify the most suitable model for deployment. The selected model is then published as a REST API, making it accessible to external clients. Additionally, the deployment is integrated with monitoring tools via the Python SDK. To streamline future use, a pipeline is configured to allow the entire process to be triggered automatically as needed.

# Key Steps

## Step 1: Model Training
### Upload dataset and register
![image](https://github.com/user-attachments/assets/74bd66ad-a224-418c-a831-ec74334ecdaa)

![image](https://github.com/user-attachments/assets/0efd387b-d74d-4d57-bfae-012914e1d2b1)

![image](https://github.com/user-attachments/assets/db4ac13a-4f11-422b-885b-251424a99954)

Creating compute cluster:
![image](https://github.com/user-attachments/assets/4eab5afd-b839-4f4a-85a5-90d051ff21f9)



### Use AutoML for finding the best classification model
Setting to classification and choice Bank data
![image](https://github.com/user-attachments/assets/cf055e6c-b100-4e42-b6b4-3ab19cbcab6a)




## Step 2: Model Deployment

### Deploy the best model

![image](https://github.com/user-attachments/assets/afd68a29-077a-4651-b8f7-befb2a3ce5f6)
![image](https://github.com/user-attachments/assets/e7bdfbba-6444-4bdf-98fc-51f4b1664c13)
![image](https://github.com/user-attachments/assets/ad6ca531-2d04-4d09-9219-e255922f386b)

![image](https://github.com/user-attachments/assets/410a7a02-94e9-420b-86b2-d51307296983)
![image](https://github.com/user-attachments/assets/26a04a96-7edf-4d40-baf1-9e6a2f276a7e)
**Running***
## Step 3: Enabled Application Insights
![image](https://github.com/user-attachments/assets/97c9d0ba-811c-4f5d-9399-b8d2735a7fa5)


Once the model is deployed, enable Application Insights to collect the service logs and useful metrics for monitoring purposes.

### Set up Python SDK for Azure in local environment

Make virtual environment in the project directory

```powershell
mkdir venv
python -m venv ./venv
```

Activate the virtual environment in current shell session

```powershell
.\venv\Scripts\activate
```

Install Azure Python ML SDK

```powershell
pip install azureml-core
```

Due to an SDK bug that caused `"Could not retrieve user token. Please run 'az login'"`, downgrading PyJWT to get around the issue:

```powershell
python -m pip install --upgrade PyJWT==1.7.1
```

@ref: https://stackoverflow.com/questions/67488064/get-workspace-failed-azuremlsdk-authenticationexception

Check current versions:

```powershell
pip show azure-core
```

```
Name: azure-core
Version: 1.17.0
```


```powershell
pip show PyJWT
```

```
Name: PyJWT
Version: 1.7.1
```

### Enabled Application Insights using Python script

![image](https://user-images.githubusercontent.com/4667129/129120622-c2e842be-4559-48c3-895e-421e0e03e62b.png)

![image](https://user-images.githubusercontent.com/4667129/129120539-27853550-8331-43ea-848a-cc313fc5f425.png)

Preview logs from the local script

![image](https://user-images.githubusercontent.com/4667129/129120654-d88f6ea1-b411-46a6-89ac-722691b53162.png)

## Step 4: View Swagger Documentation

Swagger.json URL for the endpoint is available to download

![image](https://user-images.githubusercontent.com/4667129/129433043-2e63be75-cb0e-493b-8706-4c9ff099183f.png)


Run localhost serving the edownloaded swagger.json that allows CORS for Swagger UI to access

![image](https://user-images.githubusercontent.com/4667129/129433035-1727ea70-70ca-4dd8-b765-20f84d3f8fec.png)

Run Swagger UI

![image](https://user-images.githubusercontent.com/4667129/129433076-f5ab8dbf-f4cd-4c19-9671-653e46a7d63a.png)

Access API documentation from the local swagger.json

![image](https://user-images.githubusercontent.com/4667129/129433082-52e36b54-b028-4dc3-a038-76a78ffdcbc4.png)

## Step 5: Consume Endpoint

Set up endpoint test script with URI with authentication key and sample test data

![image](https://user-images.githubusercontent.com/4667129/129434783-82c120b4-c872-4da2-869b-6542fb408c2f.png)

Test the endpoint

![image](https://user-images.githubusercontent.com/4667129/129434801-843586c9-f5fe-463e-b03a-83db5a26c16e.png)


## Step 6: Pipeline Automation

Using Python SDK in Jupyter Notebook to create a ML pipeline

![image](https://user-images.githubusercontent.com/4667129/129459282-65b3cbd4-d83c-49b1-9b96-bbe30c83d63e.png)

A pipeline endpoint is also created programatically.

![image](https://user-images.githubusercontent.com/4667129/129459356-c4481166-4e05-473a-af95-38c4311c1186.png)

The pipeline steps show the input bank marketing data set and the AutoML model where the model trainings happen. Also, the pipeline REST endpdoint is active for triggers to execute.

![image](https://user-images.githubusercontent.com/4667129/129459490-801dd325-8679-4932-aa55-b71edbe9b0d0.png)

Trigger the endpoint with proper authentication within Jupyter Notebook for a new pipeline execution and use RunDetails widget to show the step runs.

![image](https://user-images.githubusercontent.com/4667129/129459607-d795183f-fa39-4734-9f4b-9ca40abd0029.png)

The scheduled run also shows in ML Studio.

![image](https://user-images.githubusercontent.com/4667129/129460183-f73fd810-80e7-4440-872b-c8c3ad302115.png)


# Screen Recording

Video: https://youtu.be/M8rDx_4pgHk

# Future Work

Some improvements for future consideration

+ **Feature engineering**: as explained in the video, since the labels in the provided dataset are highly imbalanced (89% "No" vs 11% "Yes"), this can lead to a falsely perceived positive effect of a model's accuracy because the input data has bias towards one class. Improving this will surely be beneficial. One way is to find more data of the smaller class, or using synthetic data generation technique is also useful. 

+ **Training optimization**: increasing experiment timeout and number of cross validation for AutoML can help find better performance model.

+ **Service monitoring**: using benchmark tools such as Apache Benchmark to examine the service throughput in current compute target to see if it meets the performance target or needs to have more computation resource. Also, having the benchmark information we can estimate the acceptable time per request then set up alerts in Application Insights to detect early sign of performance degrading to proactively plan for higher compute resource. 

