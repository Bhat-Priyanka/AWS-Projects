## How to host a WordPress website using AWS?

This section explains how to host a WordPress website or blog using Amazon Web Service (AWS) using the AWS service 'Amazon Elastic Compute Cloud' (EC2).

## The main steps invoved are as below:

1.	Create a virtual machine on AWS EC2
2.	Install Apache Server
3.	Install MySQL database server
4.	Install WordPress and host it

### Explanation:

#### 1.	Create a virtual machine on AWS EC2:
1.	Log in to your AWS console and go to EC2 dashboard.
2.	Click on ‘Launch an instance.’
3.	Name the instance, for example: My First WordPress Server’.
4.	<img src="https://github.com/Bhat-Priyanka/AWS-Projects/blob/main/Host%20WordPress%20website%20on%20EC2/Images/Launch_Instance.png" width="48">
   ![alt text](https://github.com/Bhat-Priyanka/AWS-Projects/blob/main/Host%20WordPress%20website%20on%20EC2/Images/Launch_Instance.png =250x250)?raw=true)
iv.	Select Ubuntu as the operating system and keep Amazon Machine Image (AMI), Architecture and instance type as default. The default settings fall under free tier and they will not cost any money. Add pic
v.	Create new key-pair:
1.	Under ‘Key pair (login)’, click on ‘Create new key pair’.
2.	Name the key pair, for example: ‘WordPress_keypair’ and keep the default settings for key pair type and private key file format. Add pic
3.	Once clicked on ‘Create key pair’, WordPress_keypair.pem file is automatically downloaded. Keep the file in a secure location and do not share it with anyone. 
4.	After the previous step, the newly created key pair is automatically added to ‘Key pair name’ under ‘Key pair (login).’
vi.	Under ‘Network settings’, check both ‘Allow HTTPS traffic from the internet’ and ‘Allow HTTP traffic from the internet’.  Add pic
vii.	Keep the default settings for ‘Configure storage’.
viii.	Click on ‘Launch Instance’.
ix.	Once the instance is successfully created, it can be viewed under ‘Instances’ and the Instance state will be ‘Running’. Add pic
x.	Assign Elastic IP to the new instance to make it constant:
1.	Go to EC2 Dashboard and click on Elastic IP.
2.	Click on ‘Allocate Elastic IP address’ to create and allocate new elastic IP t the instance.
3.	Keep the default settings and click on ‘Allocate’. Add pic
4.	Once it is successfully created, click on ‘Actions’ under ‘Elastic IP addresses’ and select ‘Associate Elastic IP address’.
5.	Under ‘Instance’, choose the newly created instance for WordPress and click on ‘Associate’. Add pic
6.	Go back to ‘Instances’ and verify that ‘Public IPv4 address’ and ‘Elastic IP‘  have been added to the instance.

