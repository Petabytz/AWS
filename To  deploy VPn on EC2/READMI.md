
## What is AWS Site-to-Site VPN?
  By default, instances that you launch into an Amazon VPC can't communicate with your own (remote) network. You can enable access to your remote network from your VPC by attaching a virtual private gateway to the VPC, creating a custom route table, updating your security group rules, creating an AWS Site-to-Site VPN (Site-to-Site VPN) connection, and configuring routing to pass traffic through the connection. 
     Although the term VPN connection is a general term, in this documentation, a VPN connection refers to the connection between your VPC and your own on-premises network. Site-to-Site VPN supports Internet Protocol security (IPsec) VPN connections. 
     Your Site-to-Site VPN connection is either an AWS Classic VPN or an AWS VPN. For more information, see AWS Site-to-Site VPN Categories. 
     
Important
We currently do not support IPv6 traffic through a Site-to-Site VPN connection.

Concepts
The following are the key concepts for Site-to-Site VPN:

* VPN connection: A secure connection between your on-premises equipment and your VPCs.

* VPN tunnel: An encrypted link where data can pass from the customer network to or from AWS.

 Each VPN connection includes two VPN tunnels which you can simultaneously use for high availability.

 * Customer gateway: An AWS resource which provides information to AWS about your customer gateway device.

 * Customer gateway device: A physical device or software application on your side of the Site-to-Site VPN connection.

Working with Site-to-Site VPN 

You can create, access, and manage yourSite-to-Site VPN resources using any of the following interfaces:

 * AWS Management Console— Provides a web interface that you can use to access your Site-to-Site VPN resources.

 * AWS Command Line Interface (AWS CLI) — Provides commands for a broad set of AWS services, including Amazon VPC, and is supported on Windows, macOS, and Linux. For more information, see AWS Command Line Interface.

  *  AWS SDKs — Provide language-specific APIs and takes care of many of the connection details, such as calculating signatures, handling request retries, and error handling. For more information, see AWS SDKs.

  * Query API— Provides low-level API actions that you call using HTTPS requests. Using the Query API is the most direct way to access Amazon VPC, but it requires that your application handle low-level details such as generating the hash to sign the request, and error handling. For more information, see the Amazon EC2 API Reference.
  
Step 1: Create a Customer Gateway

   Create a Customer Gateway:-
   
A customer gateway provides information to AWS about your customer gateway device or software application. For more information, see Customer Gateway.

To create a customer gateway using the console

* Open the Amazon VPC console at https://console.aws.amazon.com/vpc/.

* In the navigation pane, choose Customer Gateways, and then Create Customer Gateway.

* Complete the following and then choose Create Customer Gateway:

    * (Optional) For Name, type a name for your customer gateway. Doing so creates a tag with a key of Name and the value that you specify.

    *  For Routing, select the routing type.

    *  For dynamic routing, for BGP ASN, type the Border Gateway Protocol (BGP) Autonomous System Number (ASN).

    *  For IP Address, type the static, internet-routable IP address for your customer gateway device. If your customer gateway is behind a NAT device that's enabled for NAT-T, use the public IP address of the NAT device.

To create a customer gateway using the command line or API

   * CreateCustomerGateway (Amazon EC2 Query API)

   * create-customer-gateway (AWS CLI)

   * New-EC2CustomerGateway (AWS Tools for Windows PowerShell)
   
   Step 2: Create a Virtual Private Gateway
   
   * Create a Virtual Private Gateway

When you create a virtual private gateway, you can optionally specify the private Autonomous System Number (ASN) for the Amazon side of the gateway. The ASN must be different from the BGP ASN specified for the customer gateway.

After you create a virtual private gateway, you must attach it to your VPC.

To create a virtual private gateway and attach it to your VPC
  
 * In the navigation pane, choose Virtual Private Gateways, Create Virtual Private Gateway.

 *  (Optional) Type a name for your virtual private gateway. Doing so creates a tag with a key of Name and the value that you specify.

 *  For ASN, leave the default selection to use the default Amazon ASN. Otherwise, choose Custom ASN and type a value. For a 16-bit ASN, the value must be in the 64512 to 65534 range. For a 32-bit ASN, the value must be in the 4200000000 to 4294967294 range.

  * Choose Create Virtual Private Gateway.

  * Select the virtual private gateway that you created, and then choose Actions, Attach to VPC.

  * Select your VPC from the list and choose Yes, Attach.

To create a virtual private gateway using the command line or API

  * CreateVpnGateway (Amazon EC2 Query API)

  * create-vpn-gateway (AWS CLI)

  * New-EC2VpnGateway (AWS Tools for Windows PowerShell)

To attach a virtual private gateway to a VPC using the command line or API

  * AttachVpnGateway (Amazon EC2 Query API)

  * attach-vpn-gateway (AWS CLI)

  *  Add-EC2VpnGateway (AWS Tools for Windows PowerShell)
  
Step 3: Enable Route Propagation in Your Route Table

Enable Route Propagation in Your Route Table

To enable instances in your VPC to reach your customer gateway, you must configure your route table to include the routes used by your Site-to-Site VPN connection and point them to your virtual private gateway. You can enable route propagation for your route table to automatically propagate those routes to the table for you.

For static routing, the static IP prefixes that you specify for your VPN configuration are propagated to the route table when the status of the Site-to-Site VPN connection is UP. Similarly, for dynamic routing, the BGP-advertised routes from your customer gateway are propagated to the route table when the status of the Site-to-Site VPN connection is UP.

Note

If your connection is interrupted, any propagated routes in your route table are not automatically removed. You may have to disable route propagation to remove the propagated routes; for example, if you want traffic to fail over to a static route.


To enable route propagation using the console

  * In the navigation pane, choose Route Tables, and then select the route table that's associated with the subnet; by default, this is the main route table for the VPC.

  *  On the Route Propagation tab in the details pane, choose Edit, select the virtual private gateway that you created in the previous procedure, and then choose Save.

Note

For static routing, if you do not enable route propagation, you must manually enter the static routes used by your Site-to-Site VPN connection. To do this, select your route table, choose Routes, Edit. For Destination, add the static route used by your Site-to-Site VPN connection . For Target, select the virtual private gateway ID, and choose Save.

To disable route propagation using the console

  * In the navigation pane, choose Route Tables, and then select the route table that's associated with the subnet.

  *  Choose Route Propagation, Edit. Clear the Propagate check box for the virtual private gateway, and choose Save.

To enable route propagation using the command line or API

   * EnableVgwRoutePropagation (Amazon EC2 Query API)

   * enable-vgw-route-propagation (AWS CLI)

   * Enable-EC2VgwRoutePropagation (AWS Tools for Windows PowerShell)

To disable route propagation using the command line or API

   * DisableVgwRoutePropagation (Amazon EC2 Query API)

   * disable-vgw-route-propagation (AWS CLI)

   * Disable-EC2VgwRoutePropagation (AWS Tools for Windows PowerShell)
   
Step 4: Update Your Security Group 

Update Your Security Group

To allow access to instances in your VPC from your network, you must update your security group rules to enable inbound SSH, RDP, and ICMP access.

To add rules to your security group to enable inbound SSH, RDP and ICMP access

   * In the navigation pane, choose Security Groups, and then select the default security group for the VPC.

   * On the Inbound tab in the details pane, add rules that allow inbound SSH, RDP, and ICMP access from your network, and then choose Save. For more information about adding inbound rules, see Adding, Removing, and Updating Rules in the Amazon VPC User Guide.

For more information about working with security groups using the AWS CLI, see Security Groups for Your VPC in the Amazon VPC User Guide.

Step 5: Create a Site-to-Site VPN Connection and Configure the Customer Gateway Device

Create a Site-to-Site VPN Connection and Configure the Customer Gateway Device

After you create the Site-to-Site VPN connection, download the configuration information and use it to configure the customer gateway device or software application.

To create a Site-to-Site VPN connection and configure the customer gateway

  * In the navigation pane, choose Site-to-Site VPN Connections, Create VPN Connection.

  * Complete the following information, and then choose Create VPN Connection:

  *  (Optional) For Name tag, type a name for your Site-to-Site VPN connection. Doing so creates a tag with a key of Name and the value that you specify.

  *  Select the virtual private gateway that you created earlier.

  *  Select the customer gateway that you created earlier.

  Select one of the routing options based on whether your VPN router supports Border Gateway Protocol (BGP):

 * If your VPN router supports BGP, choose Dynamic (requires BGP).

 * If your VPN router does not support BGP, choose Static. For Static IP Prefixes, specify each IP prefix for the private network of your Site-to-Site VPN connection.

  * Under Tunnel Options, you can optionally specify the following information for each tunnel:

  * A size /30 CIDR block from the 169.254.0.0/16 range for the inside tunnel IP addresses.

  * The IKE pre-shared key (PSK). The following versions are supported: IKEv1 or IKEv2.

  * For more information about these options, see Site-to-Site VPN Tunnel Options for Your Site-to-Site VPN Connection.

  * It may take a few minutes to create the Site-to-Site VPN connection. When it's ready, select the connection and choose Download Configuration.

   * In the Download Configuration dialog box, select the vendor, platform, and software that corresponds to your customer gateway device or software, and then choose Yes, Download.

To create a Site-to-Site VPN connection using the command line or API

 * CreateVpnConnection (Amazon EC2 Query API)

 * create-vpn-connection (AWS CLI)

 * New-EC2VpnConnection (AWS Tools for Windows PowerShell)
