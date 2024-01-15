## How to host a WordPress website using AWS?

This guide explains how to host a WordPress website or blog using Amazon Web Service (AWS) using the AWS service 'Amazon Elastic Compute Cloud' (EC2).

## The main steps involved are as below:

1.	Create a virtual machine on AWS EC2
2.	Install Apache Server
3.	Install MySQL database server
4.	Install WordPress and host it

### Explanation:

#### 1.	Create a virtual machine on AWS EC2:

Please follow the below steps to create a virtual machine on AWS using EC2:

1.	Log in to your AWS console and go to EC2 dashboard.
2.	Click on ‘Launch an instance.’
3.	Name the instance, for example: My First WordPress Server’.
   
   <img src="https://github.com/Bhat-Priyanka/AWS-Projects/blob/main/Host%20WordPress%20website%20on%20EC2/Images/Launch_Instance.png" width="800">
  
4.	Select Ubuntu as the operating system and keep Amazon Machine Image (AMI), Architecture and Instance type as default. The default settings fall under free tier and they will not cost any money.

   <img src="https://github.com/Bhat-Priyanka/AWS-Projects/blob/main/Host%20WordPress%20website%20on%20EC2/Images/OS_Settings.png" width="800">
   
5.	Create new key-pair:
   1. Under ‘Key pair (login)’, click on ‘Create new key pair’.
   2. Name the key pair, for example: ‘WordPress_keypair’ and keep the default settings for key pair type and private key file format.

   <img src="https://github.com/Bhat-Priyanka/AWS-Projects/blob/main/Host%20WordPress%20website%20on%20EC2/Images/Create_KeyPair.png" width="750">
      
   3. Once clicked on ‘Create key pair’, WordPress_keypair.pem file is automatically downloaded. Keep the file in a secure location and do not share it with anyone. 
   4. After the previous step, the newly created key pair is automatically added to ‘Key pair name’ under ‘Key pair (login).’
6.	Under ‘Network settings’, check both ‘Allow HTTPS traffic from the internet’ and ‘Allow HTTP traffic from the internet’.
   
   <img src="https://github.com/Bhat-Priyanka/AWS-Projects/blob/main/Host%20WordPress%20website%20on%20EC2/Images/Network_Settings.png" width="800">
   
7.	Keep the default settings for ‘Configure storage’.
8.	Click on ‘Launch Instance’.
9.	Once the instance is successfully created, it can be viewed under ‘Instances’ and the Instance state will be ‘Running’.
    
   <img src="https://github.com/Bhat-Priyanka/AWS-Projects/blob/main/Host%20WordPress%20website%20on%20EC2/Images/Instance.png" width="800">
   
10. Assign Elastic IP to the new instance to make it constant:
    1. Go to EC2 Dashboard and click on Elastic IP.
    2. Click on ‘Allocate Elastic IP address’ to create and allocate new elastic IP t the instance.
    3. Keep the default settings and click on ‘Allocate’.

       <img src="https://github.com/Bhat-Priyanka/AWS-Projects/blob/main/Host%20WordPress%20website%20on%20EC2/Images/Allocate_ElasticIP.png" width="800"> 

    5. Once it is successfully created, click on ‘Actions’ under ‘Elastic IP addresses’ and select ‘Associate Elastic IP address’.
    6. Under ‘Instance’, choose the newly created instance for WordPress and click on ‘Associate’.

       <img src="https://github.com/Bhat-Priyanka/AWS-Projects/blob/main/Host%20WordPress%20website%20on%20EC2/Images/AssociateIP.png" width="800">

    8. Go back to ‘Instances’ and verify that ‘Public IPv4 address’ and ‘Elastic IP‘  have been added to the instance.

#### 2. Install Apache Server:

Please follow the below steps to install Apache Server:

1.	If you are not working on Linux, use any of the SSH clients on Windows for example: MobaXterm, as this section needs to execute some commands. In the remote host settings under your SSH client, add public IP address of the instance.
2.	As MobaXterm is used in this guide, follow the below steps for the setup:
   1.	Open MobaXterm and click on Session.
   2.	Enter the value of your ElasticIP for ‘Remote host’.
   3.	To enter the username, go to AWS Console -> Instances and click on ‘Connect’ and here we can see that the username is ‘ubuntu’.

   <img src="https://github.com/Bhat-Priyanka/AWS-Projects/blob/main/Host%20WordPress%20website%20on%20EC2/Images/Username.png" width="800">
      
   4.	Click on ‘Advanced SSH settings’ tab and check ‘Use private key’. Add the previously downloaded WordPress_keypair.pem file. Click on ‘Ok’.
   5.	You will see that connection is successfully established to the instance on EC2.
3.	To install Apache web server, enter the following command:
   <br /> <code> sudo apt install apache2 </code>
4.	To install PHP runtime and PHP MySQL connector, enter the following command:
   <br /> <code> sudo apt install php libapache2-mod-php php-mysql </code>

#### 3. Install MySQL database server:

Please follow the below steps to install MySQL database server:

1.	To install MySQL server, enter the following command:
   <br /> <code> sudo apt install mysql-server </code>
2. Log into MySQL server using the command:
   <br /> <code> sudo mysql -u root </code>
3.	Change authentication to mysql-native-password by entering new user password:
   <br /> <code> ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password by 'Testpassword@123';  </code>
4. Create new user by using this command:
   <br /> <code> CREATE USER 'wp_user'@localhost IDENTIFIED BY 'Testpassword@123';  </code>
5.	Create a new database user for wordpress by using: 
   <br /> <code> CREATE DATABASE wp;  </code>
6. Grant all privileges on the database 'wp' to the newly created user by using this command:
   <br /> <code> GRANT ALL PRIVILEGES ON wp.* TO 'wp_user'@localhost;  </code>

#### 4. Install WordPress and host it:

Please follow the below steps to install WordPress and host it on EC2:

1.	Exit mysql by entering <code> /c </code> and exit and download WordPress using the command:
   <br /> <code> cd /tmp  </code>
   <br /> <code> Wget https://wordpress.org/latest.tar.gz  </code>
2.	Unzip WordPress package:
   <br /> <code> tar -xvf latest.tar.gz  </code>
3.	Move WordPress folder to Apache document root by this command:
   <br /> <code> sudo mv wordpress/ /var/www/html  </code>
4.	Check if WordPress is working by entering the following on your browser:
   <br /> <code> Your_Elastic_IP/wordpress  </code>
5.	You will see WordPress installation page. Click on 'Let's go'.

  	<img src="https://github.com/Bhat-Priyanka/AWS-Projects/blob/main/Host%20WordPress%20website%20on%20EC2/Images/WordPress_Install.png" width="800">

6. Configure WordPress by adding the necessary information.

   <img src="https://github.com/Bhat-Priyanka/AWS-Projects/blob/main/Host%20WordPress%20website%20on%20EC2/Images/Wordpress_config.png" width="800"> 

   1. If you get an error saying ‘unable to write to config file’, then enter the following command:
   <br /> <code> cd /var/www/html/wordpress  </code>
   2.	Create a file called wp-config.php using the command:
   <br /> <code> nano wp-config.php </code>
   3.	Paste the config rules to this file.
   4.	Go back to WordPress installation page and click on ‘Run Installation’. And enter the necessary information and click on install.

   <img src="https://github.com/Bhat-Priyanka/AWS-Projects/blob/main/Host%20WordPress%20website%20on%20EC2/Images/WordPressWelcome.png" width="800"> 

7.	If you go back to <code> Your_Elastic_IP/wordpress </code> on the browser and refresh the page, you should see that your first WordPress website is successfully hosted.

<img src="https://github.com/Bhat-Priyanka/AWS-Projects/blob/main/Host%20WordPress%20website%20on%20EC2/Images/LastPage.png" width="800"> 

### Conclusion:

By following the above steps, you will host your first WordPress website. As a next step, you can buy a custom domain name and link it to your website.



