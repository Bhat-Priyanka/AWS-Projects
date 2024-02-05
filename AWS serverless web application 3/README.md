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

<img src="https://github.com/Bhat-Priyanka/AWS-Projects/blob/main/AWS%20serverless%20web%20application%203/Images/Arch3.png" width="800">

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

#### 1. Copy the code from git repository provided by AWS to the CodeCommit repository:
   1.	Create a repository AWS Console -> AWS CodeCommit -> Create repository -> name it as ‘Wildrydes-site2’.
   2.	Add policy to IAM user to access CodeCommit.
         *	Go to AWS console -> IAM -> users -> your user account.
         *	Click on ‘Add permission’ -> ‘Attach policies directly’.
         *	Search for ‘AWSCodeCommitPowerUser’ and select it and click on ‘Next’ -> ‘Add permission’.
         *	<img src="https://github.com/Bhat-Priyanka/AWS-Projects/blob/main/AWS%20serverless%20web%20application%203/Images/AddPermission.png" width="800">
   4.	Create Git credentials for IAM user to allow HTTP requests to CodeCommit:
         *	Go to your IAM user account -> ‘Security Credentials’ -> ‘HTTP Git Credentials for AWS Code Commit’ -> ‘Generate credentials’. Copy the details or download them.
     	   * <img src="https://github.com/Bhat-Priyanka/AWS-Projects/blob/main/AWS%20serverless%20web%20application%203/Images/GitCredentials.png" width="800">
   6.	Clone the repository:
         *	Go back to CodeCommit -> Wildrydes-site2 -> ‘Clone URL’ -> ‘Clone HTTP’.
         *	Go to AWS CLI and enter the following command:
               <br /> <code> git clone https://github.com/aws-samples/aws-serverless-webapp-workshop.git </code>
            *	Enter user name and password you generated in ‘Create Git credentials’ step.
            *  Change directory to new repo by enter the following command:
               <br /> <code> cd wildrydes-site2 </code>
   7.	Copy the project code from GitHub and commit it to the new repository by enter the command:
         <br /> <code> git clone https://github.com/aws-samples/aws-serverless-webapp-workshop.git </code>
         <br /> <code> cd aws-serverless-webapp-workshop </code>
   8.	Split out WildRydesVue code into its own branch:
         <br /> <code> sudo yum install git-subtree -y </code>
         <br /> <code> git subtree split -P resources/code/WildRydesVue -b WildRydesVue </code>
   9.	Create new directory for CodeCommit repository:
         <br /> <code> mkdir ../wildrydes-site2 && cd ../wildrydes-site2 </code>
   10.	Initialize new directory:
         <br /> <code> git init </code>
   11.	Pull the WildRydesVue branch into your new repo
         <br /> <code> git pull ../aws-serverless-webapp-workshop WildRydesVue </code>
   12. Add your CodeCommit repository as a remote
         <br /> <code> git remote add origin codecommit://wildrydes-site2 </code>
   13. Push the code to your new CodeCommit repository
         <br /> <code> git push -u origin master </code>
   14. Remove the temporary local repository: 
         <br /> <code> rm -rf ../aws-serverless-webapp-workshop </code>

