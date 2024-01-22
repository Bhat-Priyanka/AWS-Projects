## How to host a serverless web application on AWS?

### Overview

AWS provides special services for running code, managing data, and integrating applications without managing any servers. Serverless services offer features like automatic scaling, high availability, and pay-for-use billing model. The goal of this project is to create a serverless web application on AWS where a user view the items from the DynamoDB table as a static website.
Required resources are downloaded from the link: <a href="https://www.linkedin.com/learning/building-serverless-applications-in-aws/building-a-serverless-application-in-aws?autoplay=true&resume=false&u=92408722" target="_blank">Link</a> -> Exercise files.

### AWS services involved with the project:
1.	Amazon S3: 
    *	Amazon Simple Store Service (S3) is an object storage service that offers scalability, data availability, security, performance, and flexible retrieval of data.
2.	AWS Lambda:
    *	Lambda is a serverless computing service that runs a code in response to events and automatically manages the resources in the fastest way.
3.	Amazon DynamoDB:
    *	DynamoDB is a serverless NOSQL database service and high performance.
4.	Amazon API Gateway:
    *	API Gateway is a fully managed service to create, publish and maintain APIs at any scale.
  
### Architecture diagram:

<img src="https://github.com/Bhat-Priyanka/AWS-Projects/blob/main/AWS%20serverless%20web%20application%202/Images/Arch2.png" width="800">

### The main steps involved are as below:

1. Create a S3 bucket and upload frontend files
2. Configure S3 bucket for static website hosting
3. Create a DynamoDB table
4. Create Lambda function
5. Set up API Gateway

### Explanation

#### 1. Create a S3 bucket and upload frontend files

1.	Go to AWS console and go to S3 and click on ‘Create bucket’.
2.	Name the bucket, for example, ‘serverlessproject-2'.

    <img src="https://github.com/Bhat-Priyanka/AWS-Projects/blob/main/AWS%20serverless%20web%20application%202/Images/CreateBucket.png" width="800">

3.	Under ‘Block Public Access settings’, uncheck ‘Block all public access’ and keep all the default settings.

    <img src="https://github.com/Bhat-Priyanka/AWS-Projects/blob/main/AWS%20serverless%20web%20application%202/Images/UnblockPublicAccess.png" width="800">

4.	After clicking on ‘Create bucket’, you will see that the bucket is created.
5.	Upload index.html and style.jss files from the link in the overview by clicking on the bucket -> ‘Upload’.

#### 2. Configure S3 bucket for static website hosting

1.	Go the new bucket -> Properties -> Static website hosting -> Edit. 
2.	In the ‘Index document’ section, type ‘index.html’ and save.

    <img src="https://github.com/Bhat-Priyanka/AWS-Projects/blob/main/AWS%20serverless%20web%20application%202/Images/StaticHosting.png" width="800">

3.	Go to ‘Permissions’ -> ‘Bucket Policy’ -> ‘Edit’ and copy and paste contents from ‘Bucket Policy.rtf’ file from the downloads. Add your bucket name.

    <img src="https://github.com/Bhat-Priyanka/AWS-Projects/blob/main/AWS%20serverless%20web%20application%202/Images/BucketPolicy.png" width="800">

#### 3. Create a DynamoDB table

1.	Go to AWS Console -> search for DynamoDB -> Create table.
2.	Name the table, for example, ‘recipes’, enter ‘id’ in the ‘Partition key’ field. Keep the default settings and create the table.

    <img src="https://github.com/Bhat-Priyanka/AWS-Projects/blob/main/AWS%20serverless%20web%20application%202/Images/CreateTable.png" width="800">
    
4.	Once the table is created, click on the table -> Actions -> Explore table items -> Create item -> Json view.
5.	From the downloads -> Recipes.rtf, enter the item one by one.

  	<img src="https://github.com/Bhat-Priyanka/AWS-Projects/blob/main/AWS%20serverless%20web%20application%202/Images/CreateItem.png" width="800">
   
   <img src="https://github.com/Bhat-Priyanka/AWS-Projects/blob/main/AWS%20serverless%20web%20application%202/Images/Items.png" width="800">

#### 4. Create Lambda function

1.	Go to AWS Console -> Lambda -> Create function.
2.	Name the function, for example, ‘getRecipes’, for ‘Runtime’, select Node.js 16.x.

    <img src="https://github.com/Bhat-Priyanka/AWS-Projects/blob/main/AWS%20serverless%20web%20application%202/Images/CreateLambda.png" width="800">

3.	Once it is created, go to ‘Code source’ and replace the code with the code present in Downloads -> Lambda code.rtf file and click on Deploy.

    <img src="https://github.com/Bhat-Priyanka/AWS-Projects/blob/main/AWS%20serverless%20web%20application%202/Images/Lambda.png" width="800">

4.	Go to Configuration -> Permissions -> click on Role name.

    <img src="https://github.com/Bhat-Priyanka/AWS-Projects/blob/main/AWS%20serverless%20web%20application%202/Images/Config.png" width="800">

5.	In the IAM console -> Permissions policies -> Add permissions -> Attach policies, search for ‘AmazonDynamoDBFullAccess’ and select it and click on ‘Add permissions’. Add pic

    <img src="https://github.com/Bhat-Priyanka/AWS-Projects/blob/main/AWS%20serverless%20web%20application%202/Images/AddPermission.png" width="800">  

#### 5. Set up API Gateway

1.	Go to AWS Console -> API Gateway -> REST API -> Build.
2.	Name the API, for example, ‘myAPI’ and click on ‘Create API’.
3.	Click on ‘Create resource’, add the name, ‘recipes’ and click on ‘Create resource’.

    <img src="https://github.com/Bhat-Priyanka/AWS-Projects/blob/main/AWS%20serverless%20web%20application%202/Images/Resource.png" width="800">

4.	Click on ‘Create method’, select GET, choose the lambda function and keep the default settings.

  	<img src="https://github.com/Bhat-Priyanka/AWS-Projects/blob/main/AWS%20serverless%20web%20application%202/Images/Method.png" width="800">

5.	Click on Deploy API -> select new stage -> name it is as ‘prod’ and click on ‘Deploy’.
6.	Go to Resources -> click on ‘/recipes’ -> Enable CORS -> select ‘GET’ for Access-Control-Allow-Methods and click on ‘Save’.

    <img src="https://github.com/Bhat-Priyanka/AWS-Projects/blob/main/AWS%20serverless%20web%20application%202/Images/CORS.png" width="800">

7.	Click on ‘Deploy API’, select ‘prod’ for stage and click on ‘Deploy’.
9.	Copy the ‘Invoke URL’ from Stages -> Stage details and go to downloads -> app.js file.
10.	In the ‘fetch()’, line, replace the URL with the invoke URL.
11.	Upload this file to the S3 bucket.
12.	Under S3 bucket -> Properties -> Static website hosting, open the URL the URL on the browser and see that the website is hosted and the recipes are displayed.

    <img src="https://github.com/Bhat-Priyanka/AWS-Projects/blob/main/AWS%20serverless%20web%20application%202/Images/Result.png" width="800">

### Conclusion

With this project, a serverless AWS application is hosted using the AWS services Amazon S3, AWS Lambda, Amazon DynamoDB, and Amazon API Gateway.
