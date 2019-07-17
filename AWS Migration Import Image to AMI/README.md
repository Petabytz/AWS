##Uploading a VirtualBox VM to an Amazon EC2 AMI
There are many blog posts about uploading a VirtualBox VM image to an AWS AMI. However, many are out of date or only cover part of the process. Below I try to describe the current easiest method to convert your custom VM to an AMI.
##Reasons for wanting a custom AMI
As part of improving test coverage I am looking into using Amazon EC2 to launch snapshots of test slaves. These snapshots will be partially configured (e.g. basic postgres, redis, and nginx configuration). It would be possible to use a standard Amazon supplied AMI and user data to perform this. However, as I want to have these VMs be as close as possible to our customer installed images, I would prefer to create a local image from our custom install media and then upload it to Amazon.
##Initial Investigation
The first step to achieving this is to search on Google. This obviously turned up the AWS documentation which shows that it is possible to upload your own AMIs.  However, it seems to mostly focus on VMware and HyperV. For my purposes, I would much prefer to use a simple VirtualBox image.

After, that I narrowed the search and found a number of blog posts describing the procedure. Many of these describe a convoluted process of creating an image, converting an image to RAW format and then uploading that.

As the AWS documentation describes it being possible to use VMDK and VHD images, and these are supported by VirtualBox, I decided to dig deeper and found that it was possible to upload these if they are created by VirtualBox.
##Steps to achieve it
###Prepare your VM
Create your VM in VirtualBox using the standard methods. The only specific requirement is that your hard disk file type should be VMDK (Virtual Machine Disk).
