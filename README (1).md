Operationalizing Machine Learning with Azure
======

# Overview

This project showcases how Azure Machine Learning can be used to build and deploy a full machine learning pipeline—from training models to serving predictions through an API in a production environment, complete with monitoring.

The dataset comes from a marketing campaign and contains information about individuals contacted via phone. The goal is to predict whether a person is likely to respond positively ("yes") and become a lead.

Dataset link: Bank Marketing Dataset

We begin by using the Azure ML Studio interface to register the dataset and set up an Automated ML experiment to train classification models.

Once the training is complete, we deploy the best-performing model as a REST API web service. Application Insights is enabled to capture telemetry data, and the API comes with a Swagger definition for documentation. The endpoint can be accessed using any HTTP client—for example, a Python script.

Next, we replicate the workflow programmatically using the Azure ML Python SDK in a Jupyter Notebook. This approach allows for greater automation, such as retraining the model automatically when the dataset or code changes. The pipeline integrates smoothly with CI/CD tools like Azure DevOps, enabling automated model training and deployment. It also offers the flexibility to deploy updated models when performance improves.

Finally, we publish the pipeline as a REST API endpoint, which can be triggered to start new experiments. A demonstration of this trigger is provided in the Jupyter Notebook.


## Architectural Diagram

This is an architecture overview of the workflow and the scope of this project.

![image](https://github.com/user-attachments/assets/3cf9948a-5c98-45d3-8620-f6e3d6989cf6)


This project involves preparing a dataset for use with Azure's Automated ML to identify the most suitable model for deployment. The selected model is then published as a REST API, making it accessible to external clients. Additionally, the deployment is integrated with monitoring tools via the Python SDK. To streamline future use, a pipeline is configured to allow the entire process to be triggered automatically as needed.

# Key Steps
 
## Step 1: Automated ML Experiment setup
### Upload dataset and register

Upload the banking data
![image](https://github.com/user-attachments/assets/edad202a-b2ae-4469-b3fb-f5ebf00c47a6)
![image](https://github.com/user-attachments/assets/f4291420-d967-4b66-8039-f0b3ca2261ab)

Set-up Compute Cluster
![image](https://github.com/user-attachments/assets/7e6c5e8d-00d8-4904-8ff8-9de9c4375c4e)
Selection DS2_V2 by recommnedation of the course
![image](https://github.com/user-attachments/assets/636fc624-9b99-4400-be62-3d986e249289)




### Use AutoML for finding the best classification model
![image](https://github.com/user-attachments/assets/47948605-5c6e-4773-950f-d679ca22cbcd)

![image](https://github.com/user-attachments/assets/1c7b0d34-c032-49ea-8191-e4f2b44b3643)
![image](https://github.com/user-attachments/assets/ffdd557d-1835-40a0-a1c8-b6b25cff6292)


## Step 2: Deploy the best Model

### Deploy the best model
![image](https://github.com/user-attachments/assets/493d07d9-6898-45f9-8af4-68b1b3b3f892)

![image](https://github.com/user-attachments/assets/045e9e6d-1975-48e8-9246-89cbd6e14f15)



## Step 3: Enabled Application Insights & Logging

First step is to ensure we have the neccesary tools in Gitbash, fx ensuring we have the right versions.

Python --Version
3.8.5

az --Version

azure cli 2.11.1

core 2.11.1

Telementru 1.0.5

Extensions:
azure-cli-ml 1.13.0

Now we have the neccesary packets we can enable the application insights.

![image](https://github.com/user-attachments/assets/6e7742d8-0c9d-4522-9729-cabf8a5d62e8)

Ensure that "service.update(enable_app_insights=True)" is set to true in the file logs.py

![image](https://github.com/user-attachments/assets/d01a2ee4-f0bb-432d-8591-03b01cdd8c26)

Run the "python logs.py" and the "Application insighs enable" will change to True in Azure.

![image](https://github.com/user-attachments/assets/85286db3-e251-451b-a77e-c122f3ae2f3c)

Giving access to insights:

![image](https://github.com/user-attachments/assets/773fe305-a5f9-4a86-9ada-7802928a6431)

Additionally, you can compare the logs.
![image](https://github.com/user-attachments/assets/ae939ceb-229b-49b4-9404-83f41bc8211e)


## Step 4: Swagger documentation

Getting the Swagger URI to connection to local host.

![image](https://github.com/user-attachments/assets/0aac6484-5d1f-4b3e-8511-e1ddda6dff51)

Make sure the file is in the same folder as Swagger.sh and Serve.py.
Either download it manualle or use wget Swagger URI.

Run Swagger.sh and Serve.py and go to local host.
Here you can access the API Documentation for the local swagger.json

![image](https://github.com/user-attachments/assets/25cd4f3b-a527-49b7-a047-d0aa78e1d041)

![image](https://github.com/user-attachments/assets/e3c4ff83-fbb7-429a-8069-dcdfc47c67d9)


## Step 5: Consume Endpoint

Edit the endpoint.py file's Scoring url and Key to match the endpoints.
Aswell as ensuring the data inputs from swagger matches the ones in the file

![image](https://github.com/user-attachments/assets/e52277ef-efea-45c5-97b1-f2d878f28d36)

Test the endpoint by running the endpoint.py

Python endpoint.py

When testing the endpoint a data.json file will be created and an result will be provided in the gitbash.

![image](https://github.com/user-attachments/assets/1afeee88-5423-4a31-957f-daa62e857133)


## Step 6: Create, publish and consime a Pipeline

Using Python SDK in Azure ML notebooks to create a ML pipeline

![image](https://github.com/user-attachments/assets/b4091236-7d18-41d5-926a-1ba0f3995b49)

When completed Creating a pipeline endpoint as well.

![image](https://github.com/user-attachments/assets/b2ee30d6-52bf-4f43-a806-a2f79033a2ef)


The pipeline steps show the input bank marketing data set and the AutoML model where the model trainings happen. Also, the pipeline REST endpdoint is active for triggers to execute.

![image](https://github.com/user-attachments/assets/79f63ee2-4ff3-4b13-9457-02f0ba7f95e4)

![image](https://github.com/user-attachments/assets/014c2ca6-ae9c-4be8-aafe-b756af2c13eb)

#####Trigger the endpoint with proper authentication within Jupyter Notebook for a new pipeline execution and use RunDetails widget to show the step runs.

![image](https://user-images.githubusercontent.com/4667129/129459607-d795183f-fa39-4734-9f4b-9ca40abd0029.png)

The scheduled run also shows in ML Studio.

![image](https://github.com/user-attachments/assets/b4091236-7d18-41d5-926a-1ba0f3995b49)


# Screen Recording

Video: https://youtu.be/M8rDx_4pgHk

# Future Work

Some improvements for future consideration

+ **Feature engineering**: as explained in the video, since the labels in the provided dataset are highly imbalanced (89% "No" vs 11% "Yes"), this can lead to a falsely perceived positive effect of a model's accuracy because the input data has bias towards one class. Improving this will surely be beneficial. One way is to find more data of the smaller class, or using synthetic data generation technique is also useful. 

+ **Training optimization**: increasing experiment timeout and number of cross validation for AutoML can help find better performance model.

+ **Service monitoring**: using benchmark tools such as Apache Benchmark to examine the service throughput in current compute target to see if it meets the performance target or needs to have more computation resource. Also, having the benchmark information we can estimate the acceptable time per request then set up alerts in Application Insights to detect early sign of performance degrading to proactively plan for higher compute resource. 

