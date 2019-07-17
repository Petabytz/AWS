## Uploading a VirtualBox VM to an Amazon EC2 AMI
There are many blog posts about uploading a VirtualBox VM image to an AWS AMI. However, many are out of date or only cover part of the process. Below I try to describe the current easiest method to convert your custom VM to an AMI.
## Reasons for wanting a custom AMI
As part of improving test coverage I am looking into using Amazon EC2 to launch snapshots of test slaves. These snapshots will be partially configured (e.g. basic postgres, redis, and nginx configuration). It would be possible to use a standard Amazon supplied AMI and user data to perform this. However, as I want to have these VMs be as close as possible to our customer installed images, I would prefer to create a local image from our custom install media and then upload it to Amazon.
## Initial Investigation
The first step to achieving this is to search on Google. This obviously turned up the AWS documentation which shows that it is possible to upload your own AMIs.  However, it seems to mostly focus on VMware and HyperV. For my purposes, I would much prefer to use a simple VirtualBox image.

After, that I narrowed the search and found a number of blog posts describing the procedure. Many of these describe a convoluted process of creating an image, converting an image to RAW format and then uploading that.

As the AWS documentation describes it being possible to use VMDK and VHD images, and these are supported by VirtualBox, I decided to dig deeper and found that it was possible to upload these if they are created by VirtualBox.
## Steps to achieve it
### Prepare your VM
Create your VM in VirtualBox using the standard methods. The only specific requirement is that your hard disk file type should be VMDK (Virtual Machine Disk).
![](https://github.com/Petabytz/AWS-Projects/blob/master/AWS%20Migration%20Import%20Image%20to%20AMI/vmdk_selection.png)
In this example, I used a dynamically allocated hard disk file of size 16GB. It should be possible to use a fixed hard disk but that would take longer to upload. For a comparison of how much space you can save, in my simple example, the 16GB dynamically allocated VMDK will be exported as a 570MB OVA file.
## Install your VM
The next step is to install the operating system on your OS and configure any required packages. For Linux, this will include an SSH server and network configured as DHCP. For more details, the Amazon documentation describes the main prerequisites and configuration requirements for VM.

Note: Some posts say you have to installing cloud-init onto the OS. This is recommended but I found that it wasn't a hard requirement for my needs.
## Export your VM
Not that we have our VM ready for use, we have to export it. In VirtualBox, select File > Export Appliance. And export your virtual machine as below.
![](https://github.com/Petabytz/AWS-Projects/blob/master/AWS%20Migration%20Import%20Image%20to%20AMI/export.png)
You now have a virtual machine in OVA format which is ready for upload to Amazon for use as an AMI. 
## Upload to Amazon
The steps to upload to Amazon are described in the AWS documentation, but I will repeat the basic steps here for completeness of this post.
### Account Permissions
As described here the your AIM user should have the following permissions

{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:ListAllMyBuckets"
      ],
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "s3:CreateBucket",
        "s3:DeleteBucket",
        "s3:DeleteObject",
        "s3:GetBucketLocation",
        "s3:GetObject",
        "s3:ListBucket",
        "s3:PutObject"
      ],
      "Resource": ["arn:aws:s3:::mys3bucket","arn:aws:s3:::mys3bucket/*"]
    }, 
    {
      "Effect": "Allow",
      "Action": [
        "iam:CreateRole",
        "iam:PutRolePolicy"
      ],
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "ec2:CancelConversionTask",
        "ec2:CancelExportTask",
        "ec2:CreateImage",
        "ec2:CreateInstanceExportTask",
        "ec2:CreateTags",
        "ec2:DeleteTags",
        "ec2:DescribeConversionTasks",
        "ec2:DescribeExportTasks",
        "ec2:DescribeInstanceAttribute",
        "ec2:DescribeInstanceStatus",
        "ec2:DescribeInstances",
        "ec2:DescribeTags",
        "ec2:ImportInstance",
        "ec2:ImportVolume",
        "ec2:StartInstances",
        "ec2:StopInstances",
        "ec2:TerminateInstances",
        "ec2:ImportImage",
        "ec2:ImportSnapshot",
        "ec2:DescribeImportImageTasks",
        "ec2:DescribeImportSnapshotTasks",
        "ec2:CancelImportTask"
      ],
      "Resource": "*"
    }
  ]
}


## Upload your image to S3
Create an S3 bucket (in the same region that you will want to run your EC2 instances in). Then upload the previously exported OVA file to that bucket.
## Install the AWS CLI
Install the AWS CLI tools as described here. I used a python virtual environment to keep the tools separate from my standard Ubuntu install.

My steps were:
$ sudo pip install venv
$ mkdir aws
$ cd aws
$ virtualenv awsvenv
$ source awsvenv/bin/activate
$ pip install awscli

You now have the AWS CLI installed and you need to configure it to work with your AMI user. You can do this by running

aws configure 
AWS Access Key ID: [your access id key]
AWS Secret Access Key: [your access key secret]
Default region name: [your region id e.g. eu-west-1]  
Default output format: [text or json]

## Create a VM Import Service Role
You have to create a role to allow you to import your VM and download images from S3. Create a file trust-policy.json

{
   "Version": "2012-10-17",
   "Statement": [
      {
         "Effect": "Allow",
         "Principal": { "Service": "vmie.amazonaws.com" },
         "Action": "sts:AssumeRole",
         "Condition": {
            "StringEquals":{
               "sts:Externalid": "vmimport"
            }
         }
      }
   ]
}

Using the AWS CLI create the role vmimport using this file.

#### aws iam create-role --role-name vmimport --assume-role-policy-document file://trust-policy.json
Create a file role-policy.json with the following policy, where disk-image-file-bucket is the bucket where the disk images are stored:


{
   "Version": "2012-10-17",
   "Statement": [
      {
         "Effect": "Allow",
         "Action": [
            "s3:ListBucket",
            "s3:GetBucketLocation"
         ],
         "Resource": [
            "arn:aws:s3:::disk-image-file-bucket"
         ]
      },
      {
         "Effect": "Allow",
         "Action": [
            "s3:GetObject"
         ],
         "Resource": [
            "arn:aws:s3:::disk-image-file-bucket/*"
         ]
      },
      {
         "Effect": "Allow",
         "Action":[
            "ec2:ModifySnapshotAttribute",
            "ec2:CopySnapshot",
            "ec2:RegisterImage",
            "ec2:Describe*"
         ],
         "Resource": "*"
      }
   ]
}

Attach the policy to the role created above.

#### aws iam put-role-policy --role-name vmimport --policy-name vmimport --policy-document file://role-policy.json

## Import the VM
You are now ready to import the OVA file you uploaded to S3 as an AMI. You can accomplish this using the AWS CLIs import-image command. Create a file containers.json describing the S3 bucket and file.

[
  {
    "Description": "RHEL 7.3 Blog OVA",
    "Format": "ova",
    "UserBucket": {
        "S3Bucket": "my-import-bucket",
        "S3Key": "ami-blogpostvm.ova"
    }
}]

#### aws ec2 import-image --description "RHEL 7.3 Blog OVA" --disk-containers file://containers.json 

RHEL 7.3 Blog OVA import-ami-fg123456 2 active pending
SNAPSHOTDETAILS 0.0 OVA
USERBUCKET my-import-bucket ami-blogpostvm.ova

Your image is now being imported. This can take a while depending on the size of the image. You can check on the status of the image using the describe-import-image-tasks command and the task id returned from the import-image command
#### aws ec2 describe-import-image-tasks --import-task-ids import-ami-fg123456
IMPORTIMAGETASKS RHEL 7.3 Blog OVA import-ami-fg123456 2 active pending
SNAPSHOTDETAILS 0.0 OVA
USERBUCKET my-import-bucket ami-blogpostvm.ova

You should see the status of the image going through the following stages
pending
converting
updating
active booting
active booted
active preparing ami
complete

(awsvenv)$ aws --output json ec2 describe-import-image-tasks --import-task-ids import-ami-fg123456
{
    "ImportImageTasks": [
        {
            "Description": "RHEL 7.3 Blog OVA",
            "LicenseType": "BYOL",
            "ImageId": "ami-aa123pp",
            "ImportTaskId": "import-ami-fg123456",
            "Status": "completed",
            "Architecture": "x86_64",
            "SnapshotDetails": [
                {
                    "SnapshotId": "snap-0845ad45ad45ad45",
                    "DeviceName": "/dev/sda1",
                    "Format": "VMDK",
                    "DiskImageSize": 570715136.0,
                    "UserBucket": {
                        "S3Key": "ami-blogpostvm.ova",
                        "S3Bucket": "my-import-bucket"
                    }
                }
            ],
            "Platform": "Linux"
        }
    ]
}

## Launch your image
Your image has now been uploaded and is available as an AMI. Using the AWS CLI, web interface, or any other tool you can launch a new instance and select your AMI.
## Note
Used JSON file editor for above Roles
