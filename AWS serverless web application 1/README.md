## How to host a serverless web application on AWS?

### Overview

AWS provides special services for running code, managing data, and integrating applications without managing any servers. Serverless services offer features like automatic scaling, high availability, and pay-for-use billing model. The goal of this project is to create a serverless web application on AWS where a user can add items to the dashboard with prices and view them.
Required resources are downloaded from the link: https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbklrWDZ5NmdVUkxsTkJNaWp4X1NXTVo2aFJvd3xBQ3Jtc0tuLWRiUm93R2cwOE5VVFpSS1Q4THVlY045eDRXc182aW5pTTVoNFYxNzdtUUZNS3U3UEpRRHFCWDAxVGxiMGF3cWtwLTVRNHQwLVlxWUZGeDc4MDd5YUloSG80TkVfUWhDZmJwNGdzLUstSzE2NjJlcw&q=https%3A%2F%2Fyoutube-code-download-32132b3.s3.amazonaws.com%2FWeb%2BApp%2Bwith%2BHTTP%2BAPI.zip&v=BFC16uM15Cg

### AWS services involved with the project:
1.	Amazon S3: 
    *	Amazon Simple Store Service (S3) is an object storage service that offers scalability, data availability, security and performance and flexible retrieval of data.
2.	AWS Lambda:
    *	Lambda is a serverless computing service that runs a code in response to events and automatically manages the resources in the fastest way.
3.	Amazon SQS:
    *	Amazon Simple Queue Service (SQS) is a fully managed message queuing service which is based on the first-in-first-out principle.
4.	Amazon DynamoDB:
    *	DynamoDB is a serverless NOSQL database service and high performance.
5.	Amazon API Gateway:
    *	API Gateway is a fully managed service to create, publish and maintain APIs at any scale.
6.	AWS CloudFormation:
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
   
2.	As the template is available in downloaded files (from the link in the overview-> API Gateway, upload aws-backend-creation-cftemplate.yaml file by selecting ‘Upload a template file’ option under ‘Specify template’.

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

#### Configure API Gateway:

1.	Go to AWS Console -> API Gateway and click on ‘items-api’ -> ‘Stages’ (Found under Deploy on left side) -> ‘Create’.
a.	Create stage:
i.	Name the stage, for example, ‘prod’ and click on ‘Create’.
2.	Create integration:
a.	Go to ‘Integrations’ (under ‘Develop’) -> ‘Manage Integration’ -> ‘Create’.
b.	Add ‘Lambda function’ as ‘Integration type’, select the Lambda function. Add pic
3.	Copy the integration ID after creating the integration.
4.	Create a route:
a.	Go to Routes (under ‘Develop’) -> ‘Create’.
b.	Create these 6 routes:
i.	GET /items
ii.	PUT /items
iii.	GET /items/{id}
iv.	DELETE /items/{id}
v.	OPTIONS /items
vi.	OPTIONS /items/{id}
5.	Attach integration:
a.	Click on each route and click on ‘Attach integration’ -> ‘Choose an existing integration and select it’. Repeat the same process for all 6 routes. Add pic
6.	Configure CORS:
a.	Go to ‘CORS’ (under ‘Develop’) -> ‘Configure’.
b.	Add the following bucket URL under ‘Access-Control-Allow-Origin’.
i.	https://serverlessexpenseapp.s3.amazonaws.com
c.	Choose ‘*’ under ‘Access-Control-Max-Methods’.
d.	Add ‘*’ under ‘Access-Control-Expose-Headers’.
e.	Add 96400 under ‘Access-Control-Max-Age’.
f.	Choose ‘YES’ for ‘Access-Control-Allow-Credentials’
g.	Click on ‘Deploy’ -> Select the newly created stage, ‘prod’ and click on ‘Deploy on stage’.
7.	Get the API Invoke URL: Go to ‘APIs’ (under API Gateway) -> click on the api -> copy of the Invoke URL of the stage name ‘prod’.
