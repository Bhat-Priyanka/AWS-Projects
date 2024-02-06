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

          	<img src="https://github.com/Bhat-Priyanka/AWS-Projects/blob/main/AWS%20serverless%20web%20application%203/Images/AddPermission.png" width="800">
   
   3.	Create Git credentials for IAM user to allow HTTP requests to CodeCommit:
         *	Go to your IAM user account -> ‘Security Credentials’ -> ‘HTTP Git Credentials for AWS Code Commit’ -> ‘Generate credentials’. Copy the details or download them.

          	<img src="https://github.com/Bhat-Priyanka/AWS-Projects/blob/main/AWS%20serverless%20web%20application%203/Images/GitCredentials.png" width="800">
   
   4.	Clone the repository:
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

#### 2.	Static website hosting:
   1.	Go to AWS Console -> AWS Amplify -> ‘Get Started’ -> ‘Host your web app’.

     	<img src="https://github.com/Bhat-Priyanka/AWS-Projects/blob/main/AWS%20serverless%20web%20application%203/Images/HostWebApp.png" width="800">
   
   2.	Select ‘AWS CodeCommit’. Select the repository ‘wildrydes-site2’ and keep the default settings.
   3.	In ‘Build Settings’ -> ‘Environment’ -> select ‘Create new environment’ and name is as ‘prod’ and click on ‘Create new role’.

     	<img src="https://github.com/Bhat-Priyanka/AWS-Projects/blob/main/AWS%20serverless%20web%20application%203/Images/BuildSettings.png" width="800">
   
   4.	Unser ‘Select trusted entity’, make sure that AWS Service is selected and under ‘Use case’, select AWS Amplify’ and click ‘Next’, keep the default settings and click on ‘Next’.
   5.	In Review page, name the role ‘ and click on ‘Create role’.
   6.	In Roles tab, search for ‘wildrydes-backend-role and click on it.
   7.	Click on ‘Add permission’ -> ‘Attach policies’ -> search for ‘AwsCodeCommitReadOnly’ and select it and click ‘Add permissions’.

     	<img src="https://github.com/Bhat-Priyanka/AWS-Projects/blob/main/AWS%20serverless%20web%20application%203/Images/AttachPolicy.png" width="800">
   
   8. Go back to ‘Review’ page, click on ‘Save and deploy’.
   9. Wait for a few mins to finish building and deploying and you can click on the URL generated to launch the web page.

      <img src="https://github.com/Bhat-Priyanka/AWS-Projects/blob/main/AWS%20serverless%20web%20application%203/Images/BuildDone.png" width="800">
       
       <img src="https://github.com/Bhat-Priyanka/AWS-Projects/blob/main/AWS%20serverless%20web%20application%203/Images/LaunchPage.png" width="800">

#### 3. User registration and authentication using AWS Cognito:
   1.	Go to AWS Console -> Cloud9 -> ‘Create environment’.
   2.	Name it as ‘webapp-development’ and once it is created, open the IDE. Keep the tab open.
   3.	Install Amplify CLI by running the command:
      <br /> <code>	npm install -g @aws-amplify/cli </code>
   4.	Clone the CodeCommit repository to Cloud9:
         1.	Copy the URL of the CodeCommit repository and clone it in Cloud9.
         2.	Make sure the repository is present in Cloud9 by entering ls command.
         3.	Navigate to wildrydes-site2 by entering:
            <br /> <code> cd wildrydes-site2 </code>
         5.	Initialize Amplify CLI by executing the command:
            <br /> <code>	Amplify init </code>
         6.	Make sure to add ‘prod’ as name for environment.

           	<img src="https://github.com/Bhat-Priyanka/AWS-Projects/blob/main/AWS%20serverless%20web%20application%203/Images/amplify.png" width="800">
         
         7.	Verify Amplify is installed by entering the command:
             <br /> <code> Amplify version </code>
   5.	 To add Cognito user pool, enter the following command:
         <br /> <code>	Amplify add auth </code>

     	 <img src="https://github.com/Bhat-Priyanka/AWS-Projects/blob/main/AWS%20serverless%20web%20application%203/Images/addAuth.png" width="800">
   
   6.	Commit code changes and start a new build in Amplify by entering the commands:
        <br /> <code> git add . </code>
        <br /> <code> git commit -m “Configure Cognito” </code>
        <br /> <code> git push </code>
   7.	Once the build is done, go to AWS console -> AWS Cognito -> check that user pool is created and an app client is created.

     	<img src="https://github.com/Bhat-Priyanka/AWS-Projects/blob/main/AWS%20serverless%20web%20application%203/Images/UserPool.png" width="800">
   
   8.	Testing user registration and authentication:
         1.	Go the browser and refresh the web page and click on ‘Giddy up!’. 
         2.	Create a new user account and confirm that you get a verification code on the email.
     
#### 4. Serverless backend with AWS Lambda and Amazon DynamoDB:
   1.	Create a new table:
         1.	Go To DynamoDB and click on ‘Create table’.
         2.	Name it as ‘Rides’ and enter ‘RideId’ for ‘Partition key’ and create the table.

           	<img src="https://github.com/Bhat-Priyanka/AWS-Projects/blob/main/AWS%20serverless%20web%20application%203/Images/Table.png" width="800">
   
   2.	Create an IAM role for Lambda function:
         1.	Go to IAM Console -> Roles -> ‘Create role’. Select ‘Lambda’ as use case.

           	<img src="https://github.com/Bhat-Priyanka/AWS-Projects/blob/main/AWS%20serverless%20web%20application%203/Images/LambdaRole.png" width="800">
         
         2.	Click next and select AWSLambdaBasicExecutionRole and check the box and click ‘Next’.
         3.	Name the role as ‘WildRydesLambda’ and create the role.
         4.	To access DynamoDB, go to the new role and Permission -> ‘Add Permission’  -> ‘Create inline policy’ and choose DynamoDB as service.
         5.	In ‘Select Actions’, choose ‘PutItem’ and check the checkbox.
         6.	In ‘Resource’, select ‘Add ARN link’.
         7.	Go to the DynamoDB table table in new tab -> Overview -> General Information’ -> ‘Additional info’ and copy the ARN link.
         8.	Paste the link in ‘Resource ARN’ field and click on ‘Add ARN’.

           	<img src="https://github.com/Bhat-Priyanka/AWS-Projects/blob/main/AWS%20serverless%20web%20application%203/Images/ARN.png" width="800">
         
         9. In ‘Review’ page, enter ‘DynamoDBWriteAccess’ as policy name and choose ‘Create policy’.
   
   3.	Create Lambda function to use IAM role:
         1.	Go to AWS Lambda Console -> ‘Create function’ and name it as ‘RequestUnicorn’. 
         2.	Select Nodejs.18x for the Runtime.
         3.	Expand ‘Change default execution role’ -> ‘Use an existing role’ and select ‘WildRydesLambda’ role and create function.

           	<img src="https://github.com/Bhat-Priyanka/AWS-Projects/blob/main/AWS%20serverless%20web%20application%203/Images/LambdaFuc.png" width="800">
         
         4.	In ‘Function code’ section, copy and paste the code from https://webapp.serverlessworkshops.io/3-serverlessbackend/4-lambda/requestUnicorn.js and click on ‘Deploy’.
   
   4.	Test the lambda function:
         1.	Go to Test tab, enter ‘TestRequirementEvent’ in event name field and enter the following code and save and click ‘Test’.
     	   <br /> <code>{
                      "path": "/ride",
                      "httpMethod": "POST",
                      "headers": {
                           "Accept": "*/*",
                           "Authorization": "eyJraWQiOiJLTzRVMWZs",
                           "content-type": "application/json; charset=UTF-8"
                        },
                      "queryStringParameters": null,
                      "pathParameters": null,
                      "requestContext": {
                        "authorizer": {
                           "claims": {
                            "cognito:username": "the_username"
                           }
                          }
                      },
                      "body": "{\"PickupLocation\":{\"Latitude\":47.6174755835663,\"Longitude\":-122.28837066650185}}"
                     } </code>
         2.	Make sure the status of ‘Execution result’ is succeeded.
     
   5.	Create API Gateway to expose Lambda function:
         1.	Create a new REST API:
               1.	Go to API Gateway Console -> ‘Create API’ -> ‘REST API’ -> ‘Build’.
               2.	Name the API as ‘WildRydes’ and click ‘Create API’.
         2.	Create Cognito User Pools Authorizer:
               1.	For newly created API, go to ‘Authorizer’ -> ‘Create new authorizer’ -> name it as ‘WildRydes’.
               2.	Select ‘Cognito’ as ‘Authorizer type’ and select ‘wildRydes’ as ‘Cognoto user pool’. Enter ‘Authorization’ as ‘Token source’ and                         create authorizer.
               3.	To test authorizer, go to the browser -> web page -> if you are logged in, you will go to /ride page and copy the token Auth Token and paste it in the ‘Test Authorizer’. Make sure you get 200 as error code and check if the details are correct.
         3.	Create a new resource and method:
               1.	Go to the API -> ‘Resources’ -> ‘Create Resource’ -> name it as ‘ride’ and select CORS.
     
            <img src="https://github.com/Bhat-Priyanka/AWS-Projects/blob/main/AWS%20serverless%20web%20application%203/Images/Resource.png" width="800">
               
               2.	Click on ‘Create method’ -> Select ‘POST’ as ‘Method type’, select ‘Lambda function’ as integration type and select ‘RequestUnicorn’ function in ‘Lambda function’ field and create method.

         <img src="https://github.com/Bhat-Priyanka/AWS-Projects/blob/main/AWS%20serverless%20web%20application%203/Images/Method.png" width="800">
         
         4.	Go to ‘Method Request’ tab -> Edit -> Add ‘wildRydes’ as authorization method and click on save.
         5.	Click on ‘Deploy API’ -> ‘Stage’ -> ‘New Stage’ -> name it as ‘prod’ and click on ‘Deploy’.
         6.	Copy the invoke URL from ‘Stage details’
     	   7.	Go to Cloud9 -> repository -> src -> config.js -> add invoke URL and save the file.
         8.	Run the following commands to commit the changes:
               <br /> <code> git add src/config.js   
               git commit -m "Configure API invokeURL"
               git push </code>

#### Testing the application:
      i.	Go to the web page, click anywhere on the map, and click on ‘Request unicorn’.
      ii.	You will see a unicorn is on its way.  You can verify that new items have been added to the DynamoDB table.

      <img src="https://github.com/Bhat-Priyanka/AWS-Projects/blob/main/AWS%20serverless%20web%20application%203/Images/Result.png" width="800">

### Conclusion:



      


   


