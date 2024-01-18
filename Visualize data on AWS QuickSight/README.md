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
    a.	To create a S3 bucket, go to AWS console and go to S3 and click on ‘Create bucket’.
    b.	Name the bucket, for example, ‘quicksightbucket’ and keep the default settings.

  	<img src="https://github.com/Bhat-Priyanka/AWS-Projects/blob/main/Host%20WordPress%20website%20on%20EC2/Images/Launch_Instance.png" width="800">
    
    c.	After clicking on ‘Create bucket’, you will see that the bucket is created.
