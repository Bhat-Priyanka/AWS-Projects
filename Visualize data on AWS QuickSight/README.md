## How to visualize data on AWS QuickSight using Amazon S3 bucket?

### Overview

The goal of this project is to get to know AWS QuickSight which is a cloud-native business intelligence service which allows you to create data visualization and also provides interactive dashboards, paginated reports, queries etc. With this project, a large dataset of 50,000 best-selling Amazon products will be visualized on QuickSight.

### The main steps to visualize data on QuickSight:

1.	Get the dataset of 50,000 best-selling Amazon products
2.	Store the dataset into an Amazon S3 bucket
3.	Connect the newly created S3 bucket with AWS QuickSight and create visualizations

### Explanation:

#### 1. Get the dataset of 50,000 best-selling Amazon products:

1.	For this project, the dataset is added from the GitHub repository: https://github.com/techwithlucy/youtube/tree/main/2-s3-quicksight.
2.	Go to the above mentioned repository and download the files ‘Amazon-Bestseller-Dataset.csv’ and ‘manifest.json’.

#### 2. Store the dataset into an Amazon S3 bucket:

1.	Create a S3 bucket:
    1.	To create a S3 bucket, go to AWS console and go to S3 and click on ‘Create bucket’.
    2.	Name the bucket, for example, ‘quicksightbucket’ and keep the default settings.

  	<img src="https://github.com/Bhat-Priyanka/AWS-Projects/blob/main/Visualize%20data%20on%20AWS%20QuickSight/Images/CreateBucket.png" width="800">
    
    3.	After clicking on ‘Create bucket’, you will see that the bucket is created.
2. Upload dataset to the bucket:
    1.  Click on the newly created bucket and upload ‘Amazon-Bestseller-Dataset.csv’ using ‘Upload’ button.
    2.	Open the downloaded file ‘manifest.json’ and replace ‘BUCKET-NAME’ with your bucket name, for example ‘quicksightbucket’.
    3.	Upload ‘manifest.json’ to the S3 bucket.
  
#### 3. Connect the newly created S3 bucket with AWS QuickSight and create visualizations:

1.	If you do not have a QuickSight subscription, sign up for a 30 days free trial version and make sure to cancel the subscription before 30 days to avoid any extra charges. Make sure to grant permission for S3 bucket.
2.	Once the account is created, click on ‘New DataSet’ to add a dataset.

    <img src="https://github.com/Bhat-Priyanka/AWS-Projects/blob/main/Visualize%20data%20on%20AWS%20QuickSight/Images/NewDataset.png" width="800">
    
3.	Select S3 as the source and enter URL of the ‘manifest.json’ file under ‘Upload a manifest file’. This can be obtained from the following: Go to S3 bucket -> manifest.json -< Copy S3 Uri.
4.	Enter any name as ‘Data source name’, for example ‘amazon data’.

    <img src="https://github.com/Bhat-Priyanka/AWS-Projects/blob/main/Visualize%20data%20on%20AWS%20QuickSight/Images/AddData.png" width="800">
    
5.	Click on ‘Visualize’ -> ‘Create’.
6.	You can see that on the left side, all fields can be viewed. 
7.	Create a visualization for the most popular brands by dragging and dropping ‘brand’ field to the ‘AutoGraph’.
8.	Once the graph is created, click on ‘brand’ -> ‘Sort Options’.

    <img src="https://github.com/Bhat-Priyanka/AWS-Projects/blob/main/Visualize%20data%20on%20AWS%20QuickSight/Images/Sort.png" width="800">

9.	Under ‘Sort by’, select ‘brand’ and ‘Sort Order’ as ‘Descending’.

    <img src="https://github.com/Bhat-Priyanka/AWS-Projects/blob/main/Visualize%20data%20on%20AWS%20QuickSight/Images/SortOptions.png" width="800">
    
10.	After clicking ‘Apply’, the results are shown as the below screenshot.

   	<img src="https://github.com/Bhat-Priyanka/AWS-Projects/blob/main/Visualize%20data%20on%20AWS%20QuickSight/Images/Result.png" width="800">

### Conclusion:
A large data set is visualized On AWS QuickSight using a S3 bucket. As a next step, we can create different visualizations based on price, title etc. 
