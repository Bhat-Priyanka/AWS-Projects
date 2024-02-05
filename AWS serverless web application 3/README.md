## How to host a serverless web application on AWS?

### Overview

AWS provides special services for running code, managing data, and integrating applications without managing any servers. Serverless services offer features like automatic scaling, high availability, and pay-for-use billing model.

The goal of this project is to create a web application that enables users to request unicorn rides from the Wild Rides fleet. The application will present users with an HTML based user interface for indicating the location where they would like to be picked up and will interface on the backend with a RESTful web service to submit the request and dispatch a nearby unicorn. The application will also provide facilities for users to register with the service and log in before requesting rides. The source code is provided by AWS.

### AWS services used in this project are:

1. AWS Amplify:
    * AWS server to build full-stack web and mobile apps on AWS.
2. Amazon Cognito:
    * User identity and access management service.
3. Cloud9:
    * Cloud based integrated developmenet environment.
5. AWS Lambda:
    * Lambda is a serverless computing service that runs a code in response to events and automatically manages the resources in the fastest way.
6.	Amazon DynamoDB:
    * DynamoDB is a serverless NOSQL database service and high performance.
6.	Amazon API Gateway:
    * API Gateway is a fully managed service to create, publish and maintain APIs at any scale.
7. AWS CodeCommit:
    * Serverless source control service that hosts Git repositories.
  
### Architecture diagram:

<img src="https://github.com/Bhat-Priyanka/AWS-Projects/blob/main/AWS%20serverless%20web%20application%202/Images/Arch2.png" width="800">

### The main steps involved are as below:

1. Copy the code from git repository provided by AWS to the CodeCommit repository 
   * In this step, a new repository is created in AWS CodeCommit by cloning the repository from GitHub. This will have all the necessary code for the project.
2. Static website hosting
   * In this step, AWS Amplify Console will be configured to host the static resources for your web application and served via Amazon CloudFront. The end users will then access the site using the public website URL exposed by AWS Amplify Console.
3. User registration and authentication using AWS Cognito
   * In this step, AWS Amplify CLI will be used to create an Amazon Cognito User Pool to manage your users accounts. We pages that enable customers to register as a new user, verify their email address, and sign into the site will be deployed.
4. Serverless backend with AWS Lambda and Amazon DynamoDB
   * In this step, AWS Lambda and Amazon DynamoDB will be used to build a backend process for handling requests from your web application. The browser application that you deployed in the first module allows users to request that a unicorn be sent to a location of their choice. In order to fulfill those requests, the JavaScript running in the browser invokes a service running in the cloud. Lambda function that will be invoked each time a user requests a unicorn. The function selects a unicorn from the fleet, records the request in a DynamoDB table, and responds to the front-end application with details about the dispatched unicorn.
   An IAM role will be created that grants Lambda function permission to write logs to Amazon CloudWatch Logs and access to write items to the DynamoDB table.
5. Create API Gateway to expose Lambda function
   * In this step, API Gateway will be used to expose the Lambda function as a RESTful API. This API will be accessible on the public Internet. It will be secured using the Amazon Cognito user pool.

### Steps to follow:


