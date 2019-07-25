## What is Amazon Lightsail?
Amazon Lightsail is the easiest way to get started with Amazon Web Services (AWS) if you just need virtual private servers.
Lightsail includes everything you need to launch your project quickly – a virtual machine, SSD-based storage, data transfer, 
DNS management, and a static IP – for a low, predictable price.

This tutorial shows you how to launch and configure a WordPress instance on Lightsail. 
It includes steps to connect to your instance by using SSH, sign in to your WordPress website, 
create a static IP and attaching it to your instance, and create a DNS zone and mapping your domain. 
When you’re done with this tutorial, you have the fundamentals to get your instance up and running on Lightsail.

These steps are covered in the following sections of this tutorial:

* Step 1: Sign up for AWS

* Step 2: Create a WordPress instance in Lightsail

* Step 3: Connect to your instance via SSH and get the password for your WordPress website

* Step 4: Sign in to the administration dashboard for your WordPress website

* Step 5: Create a Lightsail static IP address and attach it to your WordPress instance

* Step 6: Create a Lightsail DNS zone and map a domain to your WordPress instance

* Next steps

Amazon Lightsail is a VPS (Virtual Private Server) service which gives you templates to choose from, 
you can either choose a Plain OS or an OS with a pre-baked application, ready to be used with a click of a button.

Demo
Moving along in this Amazon Lightsail Tutorial, let’s launch a WordPress configured instance in Amazon Lightsa



## Step 1: Login to your AWS Management Console and select Amazon Lightsail.

![](https://github.com/Petabytz/AWS-Projects/blob/master/Launch%20and%20configure%20a%20WordPress%20instance%20in%20Amazon%20Lightsail/lightsail-select-1.png)

## Step 2: Click on the Create Instance button

![](https://github.com/Petabytz/AWS-Projects/blob/master/Launch%20and%20configure%20a%20WordPress%20instance%20in%20Amazon%20Lightsail/click-create-1.png)

## Step 3: You can now select the required option, since we are launching a WordPress server, 
we’ll select the WordPress option in the templates.

![](https://github.com/Petabytz/AWS-Projects/blob/master/Launch%20and%20configure%20a%20WordPress%20instance%20in%20Amazon%20Lightsail/select-wordpress-3.png)

## Step 4: You can leave the SSH key pair to default and move ahead and choose an instance plan according to your needs, 
we will be selecting the free tier configuration.

![](https://github.com/Petabytz/AWS-Projects/blob/master/Launch%20and%20configure%20a%20WordPress%20instance%20in%20Amazon%20Lightsail/select-plan-4.png)

## Step 5: By default, N. Virginia region, will be selected for you since Amazon Lightsail is only available in that region, 
you can choose the desired Availability Zone though.

![](https://github.com/Petabytz/AWS-Projects/blob/master/Launch%20and%20configure%20a%20WordPress%20instance%20in%20Amazon%20Lightsail/select-zone-5.png)

## Step 6: You can rename your instance to something more appropriate, and at last click on create.

![](https://github.com/Petabytz/AWS-Projects/blob/master/Launch%20and%20configure%20a%20WordPress%20instance%20in%20Amazon%20Lightsail/lightsail-createnew6.png)

It will take some time for the machine to be up and running, once everything is done, on the main instance page, click on your instance, 
and copy-paste the public IP of your instance in a new tab.

Make note of the password displayed on the screen. You use it later to sign in to your WordPress dashboard.

![]()

Sign in to the administration dashboard for your WordPress website
Now that you have the password for your WordPress dashboard, you can sign in. You can change your password, install plugins, change the theme of your website, and more. For more information, see Getting the application user name and password for your Powered by Bitnami' instance in Amazon Lightsail in the Lightsail documentation.

In a browser window, go to:

http://<public IP address>/wp-login.php
Make sure to replace <public IP address> with the public IP for your WordPress instance.

In the Username or Email Address box, enter user.

In the Password box, enter the default password obtained earlier in this tutorial.

Choose Log in.

![]()

You are now signed in to your WordPress dashboard where you can perform administrative actions on your website. For more information about administering your WordPress website, see the WordPress Codex in the WordPress documentation.



![](https://github.com/Petabytz/AWS-Projects/blob/master/Launch%20and%20configure%20a%20WordPress%20instance%20in%20Amazon%20Lightsail/click-instance-7.png)

Once you visit this IP the following page will appear.

![](https://github.com/Petabytz/AWS-Projects/blob/master/Launch%20and%20configure%20a%20WordPress%20instance%20in%20Amazon%20Lightsail/click-instance8.png)


Step 7: Create a Lightsail DNS zone and map a domain to your WordPress instance
Transfer management of your domain's DNS records to Lightsail. This allows you to more easily map a domain to your WordPress instance, and manage all of your website’s resources using the Lightsail console. For more information, see Creating a DNS zone to manage your domain’s DNS records in Amazon Lightsail in the Lightsail documentation.

* On the Networking tab of the Lightsail home page, choose Create DNS zone.
![]()

* Enter your domain, then choose Create DNS zone.
![]()

* Make note of the name server addresses listed on the page.

You add these name server addresses to your domain name’s registrar to transfer management of your domain’s DNS records to Lightsail.

![]()

After management of your domain’s DNS records are transferred to Lightsail, add an A record to point the apex of your domain to your WordPress instance, as follows:

* In the DNS zone for your domain, choose Add record.

* In the Subdomain box, enter an @ symbol to map the apex of your domain (such as example.com) to your instance. The @ symbol explicitly symbolizes that you’re adding an apex record. It is not added as a subdomain.

* In the Maps to box, choose the static IP that you attached to the WordPress instance in the previous step of this tutorial.

* Choose the save icon.
![]()

Allow time for the change to propagate through the internet's DNS before your domain begins routing traffic to your WordPress instance.




