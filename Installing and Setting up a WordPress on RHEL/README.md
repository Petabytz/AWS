## Installing and Setting up a WordPress on RHEL 7 

### Step 1: Create an AWS Account
First things first: you need to create your AWS account. 
You can sign up here. You’ll have to provide a credit card and a phone number where you will be called as part of the online registration process for verification purposes. 
Amazon offers a Free Usage Tier, which is great to explore the services and even host real apps without being charged. 
Check the details here.

### Step 2: Create an Instance
What type of EC2 instance should you use? I started my experiments with a installing and setting up a wordpress on RHEL 7 
To create a new instance, access the AWS Management Console and click the EC2 tab:

* Choose an AMI in the classic instance wizard: I chose the Basic 64-bit Amazon RHEL AMI.
* Instance details: Select the Instance Type you want to use.
* Create a new key pair. Enter a name for your key pair (i.e. Redhatkey) and download your key pair (i.e. Redhatkey.pem).
* Select the quick start security group.
* Launch your instance.

### Step 3: SSH into your Instance
Once your instance is running, you can ssh into it. First, you need to identify the address of your instance: Select the instance in the AWS Management Console, and look for the Public DNS in the instance description (bottom part of the screen).


Use that address (and a path to your .pem file) to ssh into your instance:
```bash 
 ssh -i ~/.ssh/demo.pem ec2-user@34.218.66.191
 
```
### Step 4: Install the Apache Web Server
To install the Apache Web Server, type:
```bash
yum install httpd
```
Start the Apache Web Server:
```bash
service httpd start
```
To test your Web Server, open a browser and access your web site: http://ec2-50-17-14-16.compute-1.amazonaws.com (Use your actual public DNS name). You should see a standard Amazon place holder page.

### Step 5: Install PHP
To install PHP, type:
```bash 
yum install php php-mysql
```
Restart the Apache Web Server:
```bash 
service httpd restart
```
Create a page to test your PHP installation:
```bash 
cd /var/www/html
vi test.php
```
Type i to start the insert mode
* Type <?php phpinfo() ?>
* Type :wq to write the file and quit vi
* Open a browser and access test.php to test your PHP installation: http://ec2-50-17-14-16.compute-1.amazonaws.com/test.php (Use your actual public DNS name).

### Step 6: Install MySQL
To install MySQL, type:
```bash
yum install mysql-server
```
Start MySQL:
```bash
service mysqld start
```
Create your “blog” database:
```bash
mysqladmin -uroot create blog
```
Secure your database:
```bash 
mysql_secure_Installation
```
Answer the wizard questions as follows:

* Enter current password for root: Press return for none
* Change Root Password: Y
* New Password: Enter your new password
* Remove anonymous user: Y
* Disallow root login remotely: Y
* Remove test database and access to it: Y
* Reload privilege tables now: Y

### Step 7: Install WordPress
To install WordPress, type:
```bash 
cd /var/www/html
wget http://wordpress.org/latest.tar.gz
tar -xzvf latest.tar.gzcd
```
This will uncompress WordPress in its own “wordpress” directory. I like having WordPress in a separate directory, but would rather rename it to “blog”:
```bash 
mv wordpress blog
```

Create the WordPress wp-config.php file: 
```bash
cd blog
mv wp-config-sample.php wp-config.php
vi wp-config.php
```

* Type i to start insert mode.
* Modify the database connection parameters as follows:
```bash 
define(‘DB_NAME’, ‘blog’);
define(‘DB_USER’, ‘root’);
define(‘DB_PASSWORD’, ‘YOUR_PASSWORD’);
define(‘DB_HOST’, ‘localhost’);
```
* Type :wq to write the file and quit vi

Open a Browser and access your blog: http://ec2-50-17-14-16.compute-1.amazonaws.com/blog (Use your actual public DNS name). This should trigger the WordPress configuration process.
