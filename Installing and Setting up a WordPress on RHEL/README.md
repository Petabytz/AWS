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
{ ssh -i ~/.ssh/demo.pem ec2-user@34.218.66.191
 }

```
