## Overview

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