### IPSec VPN Configuration on AWS Cloud using CloudFormation.
### Secure network connections with IPsec :

IP packages, the basic elements in internet data communication, are made up of two parts: user data like speech, text, or images, and header data containing, among others elements, the addresses of the sender and recipient. The biggest issue with these data packages, as they pass through various routers on their way to the recipient, is the fact that Internet Protocol doesn’t have encryption or authentication mechanisms. This means that data is transferred unencrypted from router to router and can be read or tampered with. And the three pillars of information security — confidentiality, authenticity, and integrity — are not guaranteed.

The Internet Protocol security, or IPsec for short, was developed in order to give the Internet Protocol vastly increased safety protection. Together, the two combine to ensure reliable security during data package transfers over open networks, which is why IPsec is an important building block for many VPN connections (virtual private network).
### What is IPSec.?

IPsec is a protocol suite, the architecture of which was suggested as a standard by the Internet Engineering Task Force (IETF). The IETF is an organization concerned with the technical advancement of the internet. IPsec was developed for the newest version of Internet Protocol (IPv6) and retrospectively also for IPv4. It can be divided into the following three function groups:

Transfer protocols: Authentication Header (AH), Encapsulating Security Payload (ESP)
Key management: Internet Security Association and Key Management Protocol (ISAKMP), Internet Key Exchange (IKE)
Database: Security Association Database (SAD), Security Policy Database (SPD)

Through the two transfer protocols, AH and ESP, IPsec guarantees the authenticity and integrity of sent data, ensuring that content from the sender reaches the recipient without being altered. For this purpose, AH offers both data origin authentication, to confirm its legitimacy, and protection for the package during transfer. Additionally, the AH protocol assigns a sequence number to the header, protecting packages from potential repeated transfer.

### The two types of IPsec: Tunnel vs Transport:

There are two existing transfer modes for secure connection with IPsec: transport mode, in which two end hosts are connected directly, and tunnel mode, which creates a connection between two IP networks.
![](https://github.com/Petabytz/AWS-Projects/blob/master/IPSec%20VPN%20Configuration%20on%20AWS%20Cloud%20using%20CloudFormation./0*S2T_pmrXjmBLu95g.png)
### Transport mode

The following occurs when IPsec is used in transport mode: the header of the transmission protocol is inserted between the IP header of the data package, which remains untouched, and the user data. Protection begins from the sender and remains throughout the transfer until the target computer is reached. Only after the package has been received is the original user data released and made available to the recipient. This means that the cryptographic and communicative terminal points are identical. Transport mode has the advantage of a very quick processing time, but only secures the user data, while the source and target addresses remain unprotected. In practical use, this mode is common for host-to-host or host-to-router connections, e.g. for network management.
### Tunnel mode

In tunnel mode, the data package receives a completely new IP header in which both the source and target address are hidden together with the user data. The header of the transfer protocol is also implemented — just as in transport mode. So it can be said that the original package is encapsulated as well. The new, outer IP header defines the cryptographic terminal point, which isn’t identical to the actual communications point stored in the inner IP header. Only once the package reaches this cryptographic end point, known as a security gateway, can it then be decrypted and forwarded on to the intended recipient. Data transfer in tunnel mode is typically carried out from gateway to gateway; host-to-gateway and host-to-host connections are also possible.
### IPv4 and IPv6 Architectures :
1. IPv4 Architecture
The protocol contains a couple aspects which caused problems with its use. These problems do not all relate to security. They are mentioned to gain a comprehensive understanding of the internet protocol and its shortcomings. The causes of problems with the protocol are:
1. Address Space
2. Routing
3. Configuration
4. Security
5. Quality of Service

    The IPv4 architecture has an address that is 32 bits wide. This limits the maximum number of computers that can be connected to the internet. The 32 bit address provides for a maximum of two billions computers to be connected to the internet.
    The problem of exceeding that number was not foreseen when the protocol was created. The small address space of the IPv4 facilitates malicious code distribution. Routing is a problem for this protocol because the routing tables are constantly increasing in size. The maximum theoretical size of the global routing tables was 2.1 million entries.
    Methods have been adopted to reduce the number of entries in the routing table. This is helpful for a short period of time, but drastic change needs to be made to address this problem.

### 2. IPv6 Architecture
When IPv6 was being developed, emphasis was placed on aspects of the IPv4 protocol that needed to be improved. The development efforts were placed in the following areas:
1. Routing and addressing
2. Multi‐protocol architecture
3. Security architecture
4. Traffic control
The IPv6 protocol’s address space was extended by supporting 128 bit addresses.
With 128 bit addresses, the protocol can support up to 3.4 * (10)³⁸ machines.
The address bits are used less efficiently in this protocol because it simplifies addressing configuration.
The IPv6 routing system is more efficient and enables smaller global routing tables.
The host configuration is also simplified. Hosts can automatically configure themselves.
This new design allows ease of configuration for the user as well as network administrator. The security architecture of the IPv6 protocol is of great interest.
 IPsec is embedded within the IPv6 protocol. IPsec functionality is the same for IPv4 and IPv6. The only difference is that IPv6 can utilize the security mechanism along the entire route.
The quality of service problem is handled with IPv6. The internet protocol allows for special handling of certain packets with a higher quality of service.
From a high‐level view, the major benefits of IPv6 are its scalability and increased security.
IPv6 also offers other interesting features that are beyond the scope of this paper. It must be emphasized that after researching IPv6 and its security features, it is not necessarily more secure than IPv4. The approach to security is only slightly better, not a radical improvement.
### What is a VPN?
![](https://github.com/Petabytz/AWS-Projects/blob/master/IPSec%20VPN%20Configuration%20on%20AWS%20Cloud%20using%20CloudFormation./0*l-14iOs8qpYLxUju.jpg)
A virtual private network (VPN) is a kind of virtual communication network that operates on the basis of a physical network. Generally, the internet is the most commonly used transport medium for VPNs. In order to ensure that data remains safe when transferred through a public cable, transport channels are secured through encryption and authentication methods. Given that data transferred via a VPN is not accessible to participants of the public network it runs on, the term “tunneling” is often used to describe this process. Following this, a VPN makes it possible to transport sensitive data via untrustworthy connections. As a result, it presents an affordable alternative to private cables.
### VPN with IPsec

‘Internet protocol security’ (IPSec) is a protocol suite designed for the internet protocol (IP) with version 6 (IPv6) and enables secure communication via non-trustworthy IP networks. Encryption and authentication mechanisms help ensure that the data traffic’s privacy, authenticity, and integrity are safeguarded. IPSec was developed with IPv6 and was retroactively specified for IPv4.
### L2TP over IPSec

A VPN implemented with LTOP over IPSec uses the ‘Layer 2 Tunneling Protocol’ (L2TP). L2TP alone doesn’t contain any encryption. For this reason, the protocol is normally combined with IPSec. While IPSec is only able to tunnel IP packets, L2TP supports a variety of packet-transferring protocols. The combination L2TP over IPSec combines the strengths of both standards. The result is a highly secure and flexible tunneling protocol.
### Step by step guide for configuring a Site-to-Site, Hardware VPN connection using AWS CloudFormation.
### Types of VPN connections

There are primarily 2 types of VPN connections :

1) Site-to-site ( S-2-S ) VPN : Site-to-site VPN’s connect entire networks to each other — for example, connecting your entire customer network to your VPC ( Virtual Private Cloud ).

2) Remote access VPN : They connect individual hosts to private networks — for example, developers working from their homes who need to access the VPC.
### Setting up a Site-to-Site, Hardware VPN connection as per the steps below.
### Step 1 : Create a VPC:
![](https://github.com/Petabytz/AWS-Projects/blob/master/IPSec%20VPN%20Configuration%20on%20AWS%20Cloud%20using%20CloudFormation./Screenshot%20from%202019-07-24%2019-25-15.png)
Enter the above code in a json file and run the “create stack” command. Poof! A VPC will be created (having the CIDR range specified) which can be checked in the web console.
### Step 2 : Create a customer gateway configuration
Once the VPC is created, lets create a customer gateway. Add the below code, to the “Resources” object.
![](https://github.com/Petabytz/AWS-Projects/blob/master/IPSec%20VPN%20Configuration%20on%20AWS%20Cloud%20using%20CloudFormation./Screenshot%20from%202019-07-24%2019-27-47.png)
Now, every time we change the stack, we will run the “update stack” command. When you run this command, a customer gateway will be created.
Important Note : There are 2 types of routing, static and dynamic. For dynamic routing, the customer gateway device should support the Border Gateway Protocol. Dynamic routing should be used for high availability across multiple VPN connections. Static routing is used when you don’t need to route your traffic via an alternate VPN connection when one VPN connection fails.

When dynamic routing is chosen, you need to provide a BgpAsn number of your customer gateway. It is not needed for static routing.

CloudFormation however has an open issue because of which, providing a BgpAsn is mandatory. The trick here is that if we need static routing, we give a default value of the BgpAsn eg. 65000.
### Step 3 : Create a virtual private gateway
![](https://github.com/Petabytz/AWS-Projects/blob/master/IPSec%20VPN%20Configuration%20on%20AWS%20Cloud%20using%20CloudFormation./Screenshot%20from%202019-07-24%2019-30-55.png)
### Step 4 : Attach the VPN Gateway created above to your VPC
![](https://github.com/Petabytz/AWS-Projects/blob/master/IPSec%20VPN%20Configuration%20on%20AWS%20Cloud%20using%20CloudFormation./Screenshot%20from%202019-07-24%2019-38-18.png)
### Step 4 : Create a VPN connection
![](https://github.com/Petabytz/AWS-Projects/blob/master/IPSec%20VPN%20Configuration%20on%20AWS%20Cloud%20using%20CloudFormation./Screenshot%20from%202019-07-24%2019-39-26.png)
The “StaticRoutesOnly” option should be set to true if you choose the static routing option. If dynamic routing is chosen, this field need not be added.

Ok, Once the above step is completed, this is what you have achieved :

Obviously, the connection will not be up until the customer gateway is configured, but this is how traffic will be routed from your customer network to your VPC. For each VPN connection, AWS provides you 2 tunnels which is their way of achieving high availability. The route table and the connection highlighted in red will be achieved via the next step ( This is the reverse routing process from your VPC back to your network ).
### Step 5 : Allow propagation of the static routes to the main route table of the VPC
![](https://github.com/Petabytz/AWS-Projects/blob/master/IPSec%20VPN%20Configuration%20on%20AWS%20Cloud%20using%20CloudFormation./Screenshot%20from%202019-07-24%2019-41-23.png)
### Step 6 : Add a static route from the customer network to your VPC
![](https://github.com/Petabytz/AWS-Projects/blob/master/IPSec%20VPN%20Configuration%20on%20AWS%20Cloud%20using%20CloudFormation./Screenshot%20from%202019-07-24%2019-42-46.png)
Now, on your AWS console, on the “VPN connections” tab, for the VPN connection you have just created there will be a “Download Configuration” file. Download the file and provide it to your network administrator. You also need to provide two other things : a) The 2 tunnel IP’s which AWS provides you and b) The entire range of IP addresses you want to access from your network. Once the customer gateway is configured, your VPN connection is up and running!
### Conclusion:

Create a subnet in your VPC and launch an instance in it. Set the ACL of the subnet and the security group of the instance to allow communication. Now, from your customer network ping a machine in your VPC using its private IP. If the ping is successful, you have setup your VPN connection!
    
