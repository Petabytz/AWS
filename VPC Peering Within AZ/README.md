## What Is Amazon VPC?
Amazon Virtual Private Cloud (Amazon VPC) enables you to launch AWS resources into a virtual network that you've defined. This virtual network closely resembles a traditional network that you'd operate in your own data center, with the benefits of using the scalable infrastructure of AWS. 

## Amazon VPC Concepts

As you get started with Amazon VPC, you should understand the key concepts of this virtual network, and how it is similar to or different from your own networks. This section provides a brief description of the key concepts for Amazon VPC.

Amazon VPC is the networking layer for Amazon EC2. If you're new to Amazon EC2, see What is Amazon EC2? in the Amazon EC2 User Guide for Linux Instances to get a brief overview. 

## VPC and Subnet Basics

A virtual private cloud (VPC) is a virtual network dedicated to your AWS account. It is logically isolated from other virtual networks in the AWS Cloud. You can launch your AWS resources, such as Amazon EC2 instances, into your VPC.

When you create a VPC, you must specify a range of IPv4 addresses for the VPC in the form of a Classless Inter-Domain Routing (CIDR) block; for example, 10.0.0.0/16. This is the primary CIDR block for your VPC. For more information about CIDR notation, see RFC 4632.

The following diagram shows a new VPC with an IPv4 CIDR block, and the main route table. 

## VPC and Subnet Sizing for IPv4

When you create a VPC, you must specify an IPv4 CIDR block for the VPC. The allowed block size is between a /16 netmask (65,536 IP addresses) and /28 netmask (16 IP addresses). After you've created your VPC, you can associate secondary CIDR blocks with the VPC. For more information, see Adding IPv4 CIDR Blocks to a VPC.

When you create a VPC, we recommend that you specify a CIDR block (of /16 or smaller) from the private IPv4 address ranges as specified in RFC 1918:

  10.0.0.0 - 10.255.255.255 (10/8 prefix)

  172.16.0.0 - 172.31.255.255 (172.16/12 prefix)

  192.168.0.0 - 192.168.255.255 (192.168/16 prefix)

You can create a VPC with a publicly routable CIDR block that falls outside of the private IPv4 address ranges specified in RFC 1918; however, for the purposes of this documentation, we refer to private IP addresses as the IPv4 addresses that are within the CIDR range of your VPC.

## Note

If you're creating a VPC for use with another AWS service, check the service documentation to verify if there are specific requirements for the IP address range or networking components.

The CIDR block of a subnet can be the same as the CIDR block for the VPC (for a single subnet in the VPC), or a subset of the CIDR block for the VPC (for multiple subnets). The allowed block size is between a /28 netmask and /16 netmask. If you create more than one subnet in a VPC, the CIDR blocks of the subnets cannot overlap.

For example, if you create a VPC with CIDR block 10.0.0.0/24, it supports 256 IP addresses. You can break this CIDR block into two subnets, each supporting 128 IP addresses. One subnet uses CIDR block 10.0.0.0/25 (for addresses 10.0.0.0 - 10.0.0.127) and the other uses CIDR block 10.0.0.128/25 (for addresses 10.0.0.128 - 10.0.0.255).

There are many tools available to help you calculate subnet CIDR blocks; for example, see http://www.subnet-calculator.com/cidr.php. Also, your network engineering group can help you determine the CIDR blocks to specify for your subnets.

The first four IP addresses and the last IP address in each subnet CIDR block are not available for you to use, and cannot be assigned to an instance. For example, in a subnet with CIDR block 10.0.0.0/24, the following five IP addresses are reserved:

  10.0.0.0: Network address.

  10.0.0.1: Reserved by AWS for the VPC router.

  10.0.0.2: Reserved by AWS. The IP address of the DNS server is always the base of the VPC network range plus two; however, we also reserve the base of each subnet range plus two. For VPCs with multiple CIDR blocks, the IP address of the DNS server is located in the primary CIDR. For more information, see Amazon DNS Server.

  10.0.0.3: Reserved by AWS for future use.

  10.0.0.255: Network broadcast address. We do not support broadcast in a VPC, therefore we reserve this address.
  
## Adding IPv4 CIDR Blocks to a VPC

You can associate secondary IPv4 CIDR blocks with your VPC. When you associate a CIDR block with your VPC, a route is automatically added to your VPC route tables to enable routing within the VPC (the destination is the CIDR block and the target is local).

In the following example, the VPC on the left has a single CIDR block (10.0.0.0/16) and two subnets. The VPC on the right represents the architecture of the same VPC after you've added a second CIDR block (10.2.0.0/16) and created a new subnet from the range of the second CIDR. 

## Getting Started with IPv4 for Amazon VPC

In this exercise, you'll create a VPC with IPv4 CIDR block, a subnet with an IPv4 CIDR block, and launch a public-facing instance into your subnet. Your instance will be able to communicate with the Internet, and you'll be able to access your instance from your local computer using SSH (if it's a Linux instance) or Remote Desktop (if it's a Windows instance).

  ![](https://docs.aws.amazon.com/vpc/latest/userguide/images/getting-started-1-diagram.png)

## Step 1: Create the VPC

In this step, you'll use the Amazon VPC wizard in the Amazon VPC console to create a VPC. The wizard performs the following steps for you:

* creates a VPC with a /16 IPv4 CIDR block (a network with 65,536 private IP addresses). For more information about CIDR notation and the sizing of a VPC, see Your VPC.
* Attaches an Internet gateway to the VPC. For more information about Internet gateways, see Internet Gateways. 
* Creates a size /24 IPv4 subnet (a range of 256 private IP addresses) in the VPC. 
* Creates a custom route table, and associates it with your subnet, so that traffic can flow between the subnet and the Internet    gateway. For more information about route tables, see Route Tables. 

## Step2 : To create a VPC using the Amazon VPC Wizard
* Open the Amazon VPC console at https://console.aws.amazon.com/vpc/home?region=us-east-1
* In the navigation bar, on the top-right, take note of the region in which you'll be creating the VPC. Ensure that you continue   working in the same region for the rest of this exercise, as you cannot launch an instance into your VPC from a different region. For more information, see Regions and Availability Zones in the Amazon EC2 User Guide for Linux Instances. 
* In the navigation pane, choose VPC dashboard. From the dashboard, choose Launch VPC Wizard. 

   ![](https://www.whizlabs.com/wp-content/uploads/2017/12/VPC-Peering-2.png)

Choose the first option, VPC with a Single Public Subnet, and then choose Select.

On the configuration page, enter a name for your VPC in the VPC name field; for example, my-vpc, and enter a name for your subnet in the Subnet name field. This helps you to identify the VPC and subnet in the Amazon VPC console after you've created them. For this exercise, you can leave the rest of the configuration settings on the page, and choose Create VPC.

(Optional) If you prefer, you can modify the configuration settings as follows, and then choose Create VPC.

* The IPv4 CIDR block displays the IPv4 address range that you'll use for your VPC (10.0.0.0/16), and the Public subnet's IPv4 CIDR field displays the IPv4 address range you'll use for the subnet (10.0.0.0/24). If you don't want to use the default CIDR ranges, you can specify your own. For more information, see VPC and Subnet Sizing.

 * The Availability Zone list enables you to select the Availability Zone in which to create the subnet. You can leave No Preference to let AWS choose an Availability Zone for you. For more information, see Regions and Availability Zones.

 * In the Service endpoints section, you can select a subnet in which to create a VPC endpoint to Amazon S3 in the same region. For more information, see VPC Endpoints.

 * The Enable DNS hostnames option, when set to Yes, ensures that instances that are launched into your VPC receive a DNS hostname. For more information, see Using DNS with Your VPC.

 * The Hardware tenancy option enables you to select whether instances launched into your VPC are run on shared or dedicated hardware. Selecting a dedicated tenancy incurs additional costs. For more information about hardware tenancy, see Dedicated Instances in the Amazon EC2 User Guide for Linux Instances.

* A status window shows the work in progress. When the work completes, choose OK to close the status window.

* The Your VPCs page displays your default VPC and the VPC that you just created. The VPC that you created is a nondefault VPC, therefore the Default VPC column displays No. 
   ![](https://www.whizlabs.com/wp-content/uploads/2017/12/VPC-Peering-3.png)
   

## Step3 :Create Subnet
 
## step 4) Now let’s create subnets in each of these VPC’s. In VPCA let’s create a subnet with a Name tag of SubnetA with a CIDR    block of 20.0.1.0/24
   
   ![](https://www.whizlabs.com/wp-content/uploads/2017/12/VPC-Peering-4.png)
   
## Step 5) Nowlet’s create a subnet in VPCB. In VPCA let’s create a subnet with a Name tag of SubnetB with a CIDR block of 10.0.1.0/24

   ![](https://www.whizlabs.com/wp-content/uploads/2017/12/VPC-Peering-5.png)
   
## Step 6) Now we are going to launch instances in both of the subnets. We want to ensure that both subnets get Public IP addresses, so enable them for the each subnet accordingly.

   ![](https://www.whizlabs.com/wp-content/uploads/2017/12/VPC-Peering-6.png)
   ![](https://www.whizlabs.com/wp-content/uploads/2017/12/VPC-Peering-7.png)
    
## Step7) Now let’s create a VPC Peering connection between both of the VPC’s

   ![](https://www.whizlabs.com/wp-content/uploads/2017/12/VPC-Peering-8.png)
   
## Step 8)In the next screen go ahead and click on Creating a new peering connection.
For the peering connection, give a name tag for the connection. And then put the requester of the VPC as VPCA

   ![](https://www.whizlabs.com/wp-content/uploads/2017/12/VPC-Peering-9.png)
   
Then choose the other VPC as the VPC to Peer with and then click on Create Peering connection.
Create VPC Connection

   ![](https://www.whizlabs.com/wp-content/uploads/2017/12/VPC-Peering-10.png)
   
## Step 9) Now the VPC Peering connection you can see that the Status is Pending Acceptance

   ![](https://www.whizlabs.com/wp-content/uploads/2017/12/VPC-Peering-11.png)
   
Choose the connection, click on Actions->Accept Request

   ![](https://www.whizlabs.com/wp-content/uploads/2017/12/VPC-Peering-12.png)
   
And then accept the request

   ![](https://www.whizlabs.com/wp-content/uploads/2017/12/VPC-Peering-13.png)
   
Once done, the connection will be in the Active State

## Step 10) Now we need to modify the route tables of both the VPC’s to ensure that communication can flow via the VPC Peering connection.

So in the Route Table for VPC B, we need to add the destination for the CIDR of VPC A as the destination and the VPC Peering connection as the target.

   ![](https://www.whizlabs.com/wp-content/uploads/2017/12/VPC-Peering.png)
   
We need to do the same thing for the Route table of VPC A

  ![](https://www.whizlabs.com/wp-content/uploads/2017/12/VPC-Peering-15.png)
If you want to connect to anyone of the instances in the VPC via the internet, ensure that the internet gateway is attached to the Route table as well.

   ![](https://www.whizlabs.com/wp-content/uploads/2017/12/VPC-Peering-16.png)
      
 ## Step 11) Now it’s time to launch instances in each VPC.
 
   ![](https://www.whizlabs.com/wp-content/uploads/2017/12/VPC-Peering-17.png)
   
Now we are going to connect to ServerA in VPCA and then ping ServerB. The private IP of ServerB is 10.0.1.138.

   ![](https://www.whizlabs.com/wp-content/uploads/2017/12/VPC-Peering-18.png)
   
Since we are going to use the ping command , ensure that the Security Group for both instances have the security group modified to allow the ICMP protocol.

   ![](https://www.whizlabs.com/wp-content/uploads/2017/12/VPC-Peering-19.png)
   
Once all of this is in place, you will be able to ping ServerB from ServerA.

   ![](https://www.whizlabs.com/wp-content/uploads/2017/12/VPC-Peering-20.png)




