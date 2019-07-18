# vyprvpn
<p>
VyprVPN is a VPN (Virtual Private Network) technology that safeguards your privacy and security with multiple encryption protocols and secure DNS. It provides fast and reliable connections with global network of servers.
</p>
<p>
  This documentation shows the step-by-step installation of VyprVPN server on AWS (Amazon Web Services).For the VyprVPN own documentation one can follow the <a href="https://support.goldenfrog.com/hc/en-us/articles/360004447451-Deploy-VyprVPN-Cloud-on-Amazon-Web-Services-AWS-"> this </a> link.
 </p>
 
 <p>
  Before getting started, first register for an account on <a href="www.vyprvpn.com"> VyprVPN </a>.
 </p>
<img src="https://gitresource.s3.us-east-2.amazonaws.com/vyprvpn/Screenshot+from+2019-07-16+16-18-54.png">
<p>
Choose any plan you want (they provide three day free trail), and confirm your e-mail,once done with account creation go to 
AWS website and log-in in your AWS account.
<p>
  1.Go to EC2 in services section. <br>
  2.Click Launch Instance and follow the below steps. <br>
  3.Choose Ubuntu server 16.04 LTS in setp 1 for choosing AMI.<br><br>
  <img src="https://gitresource.s3.us-east-2.amazonaws.com/vyprvpn/screenshots/Screenshot+from+2019-07-17+16-36-36.png">
  4.Choose t2.micro as it is in free tier.(you can opt for others also.)<br><br>
  <img src="https://gitresource.s3.us-east-2.amazonaws.com/vyprvpn/screenshots/Screenshot+from+2019-07-17+16-36-52.png"><br>
  5.In step 3 of configure instance option goto advance option and under option of "User Input" copy and paste the content of this <a href="https://www.goldenfrog.website/downloads/vyprvpn/server/cloud-init.txt">page</a>. <br><br>
  <img src="https://gitresource.s3.us-east-2.amazonaws.com/vyprvpn/screenshots/Screenshot+from+2019-07-17+16-37-32.png">
  <br>
  6.Configure your security group as given bellow:- <br>
  
  
|Type             |Protocol (auto-set)  |Port Range   |Source|
|:---------------:|:-------------------:|:-----------:|:------:|
|HTTP	            |TCP	                |80	          |My IP (will detect and select your IP)|
|HTTPS	          |TCP	                |443	        |My IP|
|SSH	            |TCP	                |22	          |My IP|
|Custom TCP Rule	|TCP	                |222	        |My IP|
|Custom UDP Rule	|UDP	                |443	        |Anywhere|
|Custom UDP Rule	|UDP	                |500	        |Anywhere|
|Custom UDP Rule	|UDP	                |1194	        |Anywhere|
|Custom UDP Rule	|UDP	                |1701	        |Anywhere|
|Custom UDP Rule	|UDP	                |4500	        |Anywhere|
|Custom UDP Rule	|UDP	                |15001-20000	|Anywhere|
|Custom UDP Rule	|UDP	                |25001-30000	|Anywhere|

<br>
7.Finally launch the instance by either creating or using previous key-pair.<br><br>
<img src="https://gitresource.s3.us-east-2.amazonaws.com/vyprvpn/screenshots/Screenshot+from+2019-07-17+16-41-27.png"><br>
8.Copy your public IP address and open it in browser ,you will see the VyprVPN installation screen. <br><br>
<img src="https://gitresource.s3.us-east-2.amazonaws.com/vyprvpn/screenshots/Screenshot+from+2019-07-17+16-41-37.png"><br><br>

9.After the Vyprvpn Server Installation is completed ,log-in  with the below credentials :- <br>
```bash
Username: admin
Password: Grod42decK
```
<br>
