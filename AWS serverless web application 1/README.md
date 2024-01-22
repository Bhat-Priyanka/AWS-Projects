## How to host a serverless web application on AWS?

### Overview

AWS provides special services for running code, managing data, and integrating applications without managing any servers. Serverless services offer features like automatic scaling, high availability, and pay-for-use billing model. The goal of this project is to create a serverless web application on AWS where a user can add items to the dashboard with prices and view them.
Required resources are downloaded from the link: <a href="https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbklrWDZ5NmdVUkxsTkJNaWp4X1NXTVo2aFJvd3xBQ3Jtc0tuLWRiUm93R2cwOE5VVFpSS1Q4THVlY045eDRXc182aW5pTTVoNFYxNzdtUUZNS3U3UEpRRHFCWDAxVGxiMGF3cWtwLTVRNHQwLVlxWUZGeDc4MDd5YUloSG80TkVfUWhDZmJwNGdzLUstSzE2NjJlcw&q=https%3A%2F%2Fyoutube-code-download-32132b3.s3.amazonaws.com%2FWeb%2BApp%2Bwith%2BHTTP%2BAPI.zip&v=BFC16uM15Cg" target="_blank">Link</a>



### AWS services involved with the project:
1.	Amazon S3: 
    *	Amazon Simple Store Service (S3) is an object storage service that offers scalability, data availability, security, performance, and flexible retrieval of data.
2.	AWS Lambda:
    *	Lambda is a serverless computing service that runs a code in response to events and automatically manages the resources in the fastest way.
3.	Amazon DynamoDB:
    *	DynamoDB is a serverless NOSQL database service and high performance.
4.	Amazon API Gateway:
    *	API Gateway is a fully managed service to create, publish and maintain APIs at any scale.
5.	AWS CloudFormation:
    *	CloudFormation is a tool to model, provision and manage code.
  
### Architecture diagram:

<img src="https://github.com/Bhat-Priyanka/AWS-Projects/blob/main/AWS%20serverless%20web%20application%201/Images/AWSArch.png" width="800">

### The main steps involved are as below:

1. Create a stack on CloudFormation
2. Create a S3 bucket
3. Configure API Gateway
4. Client-side coding

### Explanation

#### 1. Create a stack on CloudFormation:

In this section we will create Lambda function, API and DynamoDB table within a CloudFormation template and upload it to CloudFormation.

Create a stack on CloudFormation:
1.	Go to AWS console -> CloudFormation and create a stack.

  	<img src="https://github.com/Bhat-Priyanka/AWS-Projects/blob/main/AWS%20serverless%20web%20application%201/Images/CreateStack.png" width="800">
   
2.	As the template is available in downloaded files (from the link in the overview) -> API Gateway, upload aws-backend-creation-cftemplate.yaml file by selecting ‘Upload a template file’ option under ‘Specify template’.

   <img src="https://github.com/Bhat-Priyanka/AWS-Projects/blob/main/AWS%20serverless%20web%20application%201/Images/UploadTemplate.png" width="800">

3.	After uploading, if you click on ‘View on designer’, you can visualize what is going to be created.

  	<img src="https://github.com/Bhat-Priyanka/AWS-Projects/blob/main/AWS%20serverless%20web%20application%201/Images/Designer.png" width="800">
   
4.	Click on ‘Create stack’ button to go back and create the stack. (Marked in the previous screenshot.)
5.	Click on ‘Next’ and add a name for the stack for example, 'ExpenseApp'.

   <img src="https://github.com/Bhat-Priyanka/AWS-Projects/blob/main/AWS%20serverless%20web%20application%201/Images/StackName.png" width="800">
   
6.	Keep default settings in the ‘Configuration’ page and click on ‘Next’.
7.	In the ‘Review ExpenseApp’ page, click on ‘I acknowledge’ checkbox and click on ‘Submit’. 
8.	All events are created as shown in the screenshot.

   <img src="https://github.com/Bhat-Priyanka/AWS-Projects/blob/main/AWS%20serverless%20web%20application%201/Images/Events.png" width="800">

#### 2. Create a S3 bucket:

1.	Go to AWS console and go to S3 and click on ‘Create bucket’.
2.	Name the bucket, for example, ‘serverlessexpenseapp’ and keep the default settings.
3.	After clicking on ‘Create bucket’, you will see that the bucket is created.
4.	Go to the newly created bucket and go to Properties -> ‘Static website hosting’ and click on ‘Edit’.
5.	Enable static website hosting and add the name of the index document.

   <img src="https://github.com/Bhat-Priyanka/AWS-Projects/blob/main/AWS%20serverless%20web%20application%201/Images/EditHosting.png" width="800">
   
6.	Go back to the bucket -> Permissions -> ‘Block public access’ -> Edit.
7.	Unblock all public access by clicking on the checkbox.
8.	Go back to the bucket -> Permissions -> Block policy -> Edit and add the following policy.
   <br /> <code> {
                     "Version": "2012-10-17",
                     "Statement": [
                        {
                           "Sid": "PublicReadGetObject",
                           "Effect": "Allow",
                           "Principal": "*",
                           "Action": "s3:GetObject",
                           "Resource": "arn:aws:s3:::serverlessexpenseapp/*"
                        }
                     ]
                  } </code>
                  
   <img src="https://github.com/Bhat-Priyanka/AWS-Projects/blob/main/AWS%20serverless%20web%20application%201/Images/BucketPolicy.png" width="800">

9.	Go back to the bucket -> Permissions -> Cross-origin resource sharing (CORS) -> Edit and add the following:
   <br /> <code> [
                     {
                        "AllowedHeaders": [
                           "*"
                        ],
  	                     "AllowedMethods": [
                           "GET"
                        ],
                        "AllowedOrigins": [
                           "*"
                        ],
                        "ExposeHeaders": []
                     }
                  ] </code>

   <img src="https://github.com/Bhat-Priyanka/AWS-Projects/blob/main/AWS%20serverless%20web%20application%201/Images/CORS.png" width="800">              

#### 3. Configure API Gateway:

1. Create stage:
   *    Go to AWS Console -> API Gateway and click on ‘items-api’ -> ‘Stages’ (found under Deploy on left side) -> ‘Create’.
   *    Name the stage, for example, ‘prod’ and click on ‘Create’.

   *    <img src="https://github.com/Bhat-Priyanka/AWS-Projects/blob/main/AWS%20serverless%20web%20application%201/Images/Stage.png" width="800">
      
2.	Create integration:
   	*	Go to ‘Integrations’ (under ‘Develop’) -> ‘Manage Integration’ -> ‘Create’.
   	*	Add ‘Lambda function’ as ‘Integration type’, select the Lambda function.
   	*	Copy the integration ID after creating the integration.
3.	Create a route:
   	*	Go to Routes (under ‘Develop’) -> ‘Create’.
   	*	Create these 6 routes:
        *	GET /items
        *	PUT /items
        *	GET /items/{id}
        *	DELETE /items/{id}
        *	OPTIONS /items
        *	OPTIONS /items/{id}
     	*	<img src="https://github.com/Bhat-Priyanka/AWS-Projects/blob/main/AWS%20serverless%20web%20application%201/Images/Routes.png" width="800">
      
4.	Attach integration:
   	*	Click on each route and click on ‘Attach integration’ -> ‘Choose an existing integration and select it’. Repeat the same process for all 6 routes.

        <img src="https://github.com/Bhat-Priyanka/AWS-Projects/blob/main/AWS%20serverless%20web%20application%201/Images/AttachIntegration.png" width="800">

 	<img src="https://github.com/Bhat-Priyanka/AWS-Projects/blob/main/AWS%20serverless%20web%20application%201/Images/Integration.png" width="800">	
   
5.	Configure CORS:
        *	Go to ‘CORS’ (under ‘Develop’) -> ‘Configure’.
   	*	Add the following bucket URL under ‘Access-Control-Allow-Origin’.
      		*	https://serverlessexpenseapp.s3.amazonaws.com
  	*	Choose ‘*’ under ‘Access-Control-Max-Methods’.
     	*	Add ‘*’ under ‘Access-Control-Expose-Headers’.
   	*	Add 96400 under ‘Access-Control-Max-Age’.
   	*	Choose ‘YES’ for ‘Access-Control-Allow-Credentials’

   		<img src="https://github.com/Bhat-Priyanka/AWS-Projects/blob/main/AWS%20serverless%20web%20application%201/Images/CORSConfig.png" width="800">
   
   	*	Click on ‘Deploy’ -> Select the newly created stage, ‘prod’ and click on ‘Deploy on stage’.
6.	Get the API Invoke URL: Go to ‘APIs’ (under API Gateway) -> click on the api -> copy of the Invoke URL of the stage name ‘prod’.

#### 4. Client-side coding:
1.	Install Node.js -> https://nodejs.org/en/download
2.	Go to downloads -> API Gateway folder -> Code -> client -> src -> config.ts and add apiId from the invoke URL.
3.	For example, if <br /> <code> https://h9s9gchnv6.execute-api.eu-north-1.amazonaws.com/prod </code> is your invoke URL, then <br /> <code> h9s9gchnv6 </code> is the apiId.
4.	Open terminal in the directory -> Code -> client and type the following command to install the necessary dependencies:
	<br /> <code> npm install </code>
5.	Create the build by typing:
    	<br /> <code> npm run build </code>
6.	Upload the build files to the S3 bucket by selecting everything under ‘Build’ directory and dragging and dropping them to the S3 bucket.
7.	Click on ‘index.html’ file and copy the ‘Object URL’ and paste it into the browser. 
8.	You can see that the website is hosted successfully and you can add items and verify that they have been added to DynamoDB successfully.

   	<img src="https://github.com/Bhat-Priyanka/AWS-Projects/blob/main/AWS%20serverless%20web%20application%201/Images/Result.png" width="800">

### Conclusion

With this project, a serverless AWS application is hosted using the AWS services Amazon S3, AWS Lambda, Amazon DynamoDB, Amazon API Gateway, and AWS CloudFormation.
