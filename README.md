# ML-Workflow-for-Scones-Unlimited
The primary objective of this project was to build and deploy an image classification model for Scones Unlimited, a scone-delivery-focused logistic company, using AWS SageMaker.

## Overview

Image Classifiers are used in the field of computer vision to identify the content of an image and it is used across a broad variety of industries, from advanced technologies like autonomous vehicles and augmented reality, to E-Commerce platforms, and even in diagnostic medicine.

For the hypothetical company <b>Scones Unlimited</b>, the image classification model can help the team in a variety of ways in their operating environment: detecting people and vehicles in video feeds from roadways, better support routing for their engagement on social media, detecting defects in their scones, and many more!

In this project, we built an `image classification model` that can automatically detect which kind of vehicle delivery drivers have, in order to route them to the correct loading bay and orders. Assigning delivery professionals who have a bicycle to nearby orders and giving motorcyclists orders that are farther can `help Scones Unlimited optimize their operations`.

Our goal is to ship a `scalable and safe model`. Once our model becomes available to other teams on-demand, it’s important that our model can scale to meet demand, and that safeguards are in place to monitor and control for drift or degraded performance.

This project uses `AWS Sagemaker` to build an image classification model that can tell bicycles apart from motorcycles. The model has been deployed using `AWS Lambda functions` to build supporting services, and `AWS Step Functions` to compose the model and services into an event-driven application.

## Project Steps
1) Data staging
2) Model training and deployment
3) Lambdas and step function workflow
4) Testing and evaluation
5) Optional challenge
6) Cleanup cloud resources

## Project File Details
1) `starter.ipynb`: Jupyter notebook showcases a machine learning working workflow for Image Classification. This includes the necessary preprocessing of the scones unlimited image dataset, model training, deployment and monitor using Amazon SageMaker and other associated AWS Services.

2) `starter.html`: Web-page displaying `starter.ipynb`.

3) `lambda.py`: Compilation of the necessary 'lambda.py' scripts used by three AWS Lambda functions to create a Step Functions workflow. <i>(<b>Note</b>: The 'lambda.py' file typically has a 'lambda_handler' function, which acts as the entry point for the Lambda function when it is triggered by an event such as an HTTP request or a scheduled cron job. This function takes an 'event' object, which contains information about the triggering event and a 'context' object, which contains information about the current execution environment. The 'lambda_handler' function is where the main logic of the Lambda function is executed, it can interact with other AWS services, perform calculations or process data. The function can also return a response to the service or client that triggered the Lambda function.)</i>

4) `step-function.json`: Step Function exported to JSON

5) `Step Function Workflow Images` directory: Contains screenshots of working and failing step functions along with their execution workflows.

## Dependencies
1) Python 3 (Data Science) - v3.7.10 kernel
2) ml.t3.medium instance
3) Python 3.8 runtime for the AWS Lambda Functions

## Lambda Functions

The Python scripts for these lambda functions is present in `lambda.py` file.

- The `serializeImageData` Lambda Function takes the address of an image hosted in S3, and returns a serialized JSON object.
- The `classifyImages` Lambda Function accepts the JSON object obtained from step 1 and sends it to an endpoint for the trained model to classify the image and collects inferences as a JSON object. Since the python script involves dependencies such as SageMaker SDK, it has to be imported to `AWS Lambda` as a dependency package to set up this function. (Refer to [AWS Docs](https://docs.aws.amazon.com/lambda/latest/dg/python-package.html#python-package-create-with-dependency) for more details.)
- The `filterInferences` Lambda Function takes the inference data from step 2, and filters only the images that meet the pre-defined threshold.

These Lambda functions are then used to create our Step Function, as given in `step-function.json`.

## Building a State Machine via AWS Step Functions

### Step Function
![Graph](./Step%20Function%20Workflow%20Images/Step%20Function.PNG)

### Step Function Execution Workflow
![Execution Workflow](./Step%20Function%20Workflow%20Images/Execution%20Flow%20of%20Step%20Function.PNG)

### Working Step Function
![Working](./Step%20Function%20Workflow%20Images/Working%20Step%20Function.PNG)

### When threshold requirements are not met
#### Step Function Fails
![Failed function](./Step%20Function%20Workflow%20Images/Threshold%20Not%20Met.PNG)

#### Execution Workflow when Step Function Fails
![Execution Workflow when Step Function Fails](./Step%20Function%20Workflow%20Images/Threshold%20Not%20Met%20Execution%20Flow.PNG)