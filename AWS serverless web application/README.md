## How to host a serverless web application on AWS?

### Overview

AWS provides special services for running code, managing data, and integrating applications without managing any servers. Serverless services offer features like automatic scaling, high availability, and pay-for-use billing model. The goal of this project is to create a serverless web application on AWS where a user can add items to the dashboard with prices and view them.
Required resources are downloaded from the link: https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbklrWDZ5NmdVUkxsTkJNaWp4X1NXTVo2aFJvd3xBQ3Jtc0tuLWRiUm93R2cwOE5VVFpSS1Q4THVlY045eDRXc182aW5pTTVoNFYxNzdtUUZNS3U3UEpRRHFCWDAxVGxiMGF3cWtwLTVRNHQwLVlxWUZGeDc4MDd5YUloSG80TkVfUWhDZmJwNGdzLUstSzE2NjJlcw&q=https%3A%2F%2Fyoutube-code-download-32132b3.s3.amazonaws.com%2FWeb%2BApp%2Bwith%2BHTTP%2BAPI.zip&v=BFC16uM15Cg

#### AWS services involved with the project:
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
  
#### Architecture diagram:

