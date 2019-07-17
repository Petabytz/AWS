## What Is Amazon VPC?
Amazon Virtual Private Cloud (Amazon VPC) enables you to launch AWS resources into a virtual network that you've defined. This virtual network closely resembles a traditional network that you'd operate in your own data center, with the benefits of using the scalable infrastructure of AWS. 

## Amazon VPC Concepts

As you get started with Amazon VPC, you should understand the key concepts of this virtual network, and how it is similar to or different from your own networks. This section provides a brief description of the key concepts for Amazon VPC.

Amazon VPC is the networking layer for Amazon EC2. If you're new to Amazon EC2, see What is Amazon EC2? in the Amazon EC2 User Guide for Linux Instances to get a brief overview. 

## Getting Started with IPv4 for Amazon VPC

In this exercise, you'll create a VPC with IPv4 CIDR block, a subnet with an IPv4 CIDR block, and launch a public-facing instance into your subnet. Your instance will be able to communicate with the Internet, and you'll be able to access your instance from your local computer using SSH (if it's a Linux instance) or Remote Desktop (if it's a Windows instance).

![](https://docs.aws.amazon.com/vpc/latest/userguide/images/getting-started-1-diagram.png)

## Step 1: Create the VPC

In this step, you'll use the Amazon VPC wizard in the Amazon VPC console to create a VPC. The wizard performs the following steps for you:

* creates a VPC with a /16 IPv4 CIDR block (a network with 65,536 private IP addresses). For more information about CIDR notation and the sizing of a VPC, see Your VPC.
* Attaches an Internet gateway to the VPC. For more information about Internet gateways, see Internet Gateways. 
* Creates a size /24 IPv4 subnet (a range of 256 private IP addresses) in the VPC. 
* Creates a custom route table, and associates it with your subnet, so that traffic can flow between the subnet and the Internet    gateway. For more information about route tables, see Route Tables. 
## To create a VPC using the Amazon VPC Wizard
* Open the Amazon VPC console at ![](https://console.aws.amazon.com/vpc/.)
* In the navigation bar, on the top-right, take note of the region in which you'll be creating the VPC. Ensure that you continue   working in the same region for the rest of this exercise, as you cannot launch an instance into your VPC from a different region. For more information, see Regions and Availability Zones in the Amazon EC2 User Guide for Linux Instances. 
* In the navigation pane, choose VPC dashboard. From the dashboard, choose Launch VPC Wizard. 

![](https://www.whizlabs.com/wp-content/uploads/2017/12/VPC-Peering-2.png)
