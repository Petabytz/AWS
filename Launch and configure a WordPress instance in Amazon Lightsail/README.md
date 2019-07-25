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

![]()

## Step 3: You can now select the required option, since we are launching a WordPress server, 
we’ll select the WordPress option in the templates.

![]()

## Step 4: You can leave the SSH key pair to default and move ahead and choose an instance plan according to your needs, 
we will be selecting the free tier configuration.

![]()

## Step 5: By default, N. Virginia region, will be selected for you since Amazon Lightsail is only available in that region, 
you can choose the desired Availability Zone though.

![]()

## Step 6: You can rename your instance to something more appropriate, and at last click on create.

![]()

It will take some time for the machine to be up and running, once everything is done, on the main instance page, click on your instance, 
and copy-paste the public IP of your instance in a new tab.

![]()

Once you visit this IP the following page will appear.


