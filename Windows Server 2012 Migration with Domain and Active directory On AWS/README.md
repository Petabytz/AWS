## Server Configuration Basic:-
### Install Active Directory on Windows Server 2012
### Use the following steps to install Active Directory on the server:
1.Open the Server Manager from the taskbar.
2.From the Server Manager dashboard, select Add roles and features.
The Roles and Features Wizard launches. This wizard enables you to make modifications to the Windows Server 2012 instance.
3. On the Installation Type screen, select Role-based or feature-based and click Next.
Note: Roles represent the major feature sets of the server, such as Internet Information Services (IIS). Features provide additional functionality for a given role.
4. By default, the current server is selected. Click Next.
5. On the Server Roles screen, select the checkbox next to Active Directory Domain Services.
A notice displays that it explains that you must also install additional roles, services, or features in order to install Domain Services. These additional capabilities include certificate services, federation services, lightweight directory services, and rights management. To select additional capabilities, click Add Features.
6. On the Select features screen, select the checkboxes next to the features that you want to install during the AD DS installation process and click Next.
![](https://github.com/Petabytz/AWS-Projects/blob/master/Windows%20Server%202012%20Migration%20with%20Domain%20and%20Active%20directory%20On%20AWS/0_64iz65t0mrEhjFIu.png)
7. Review the information on the AD DS tab, then click Next.
8. Review the information on the Confirm installation selections screen, then click Install.
#### Note: Information on the progress of the installation displays. After the installation is complete, the AD DS role displays on the Server Manager landing page.
## Start the remote registry service:-
Before you can promote the server to a domain controller, you must start the remote registry service by using the following steps:
1.Click Start > Control Panel.
2.Under Services, right-click Remote Registry and open the Properties menu.
3.From the Startup type: drop-down menu, select Automatic.
4.Under Service Status, select Start. the remote registry service starts.
## Configure Active Directory:-
After you have installed the AD DS role, you must configure the server for your domain by using the following steps:
1.From the taskbar, click Open the Server Manager.
2.Select the yellow notifications icon in the top navigation bar of the Server Manager window.
The Notifications Pane opens and displays a Post-deployment Configuration notification. Click the Promote this server to a domain controller link that appears in the notification.
![](https://github.com/Petabytz/AWS-Projects/blob/master/Windows%20Server%202012%20Migration%20with%20Domain%20and%20Active%20directory%20On%20AWS/promote-8c761b9f1f5d5acef2dc71040447cd485ba712d9ef6e9044afa235d33c6bacc4.png)
3. From the Deployment Configuration tab, select Radial options > Add a new forest. Enter your root domain name in the Root domain name field and click Next.
4. Select a Domain and a Forest functional level.
Note: These selections affect features and server domain controller eligibility. For further information on domains and forest functional levels, see the official Microsoft documentation.
Enter a password for Directory Services Restore Mode (DSRM) in the Password field.
Note: The DSRM password is used when booting the Domain Controller into recovery mode.
5. Review the warning on the DNS Options tab and select Next.
6. Confirm or enter a NetBIOS name and click Next.
7. Specify the locations of the Database, Log files, and SYSVOL folders, then click Next.
8. Review the configuration options and click Next.
9. The system checks if all of the necessary prerequisites are installed on the system. If the system passes these checks, click Install.
Note: The server automatically reboots after the installation is complete.
10. After the server reboots, reconnect to it by using the Microsoft Remote Desktop Protocol (RDP)
## Convert Windows 2012 VM Image To .OVA Format(Import Image):-
1.Go to File Tab
![](https://github.com/Petabytz/AWS-Projects/blob/master/Windows%20Server%202012%20Migration%20with%20Domain%20and%20Active%20directory%20On%20AWS/1_wgOxXRt3lztnTDPJsLkClA.png)
2. In File Tab click the Export button
3. Click Next button
![](https://github.com/Petabytz/AWS-Projects/blob/master/Windows%20Server%202012%20Migration%20with%20Domain%20and%20Active%20directory%20On%20AWS/1_WE5-Zx6JqDJzRzhZHkFldg.png)
4. Select location for saving image
![](https://github.com/Petabytz/AWS-Projects/blob/master/Windows%20Server%202012%20Migration%20with%20Domain%20and%20Active%20directory%20On%20AWS/1_G76tIunxg9KRmZNR0235Fg.png)
5. And Click the Export button
6. Create an S3 bucket using AWS CLI
aws s3 mb s3://<Bucket Name>
7. Copy .OVA File Local Location to S3 Bucket using AWS CLI
aws s3 cp <Location of.OVA file> s3://<Bucket Name>
8. After Copy Create IAM Role for Migration
Note:- creating roll with EC2 Full Accesses
![](https://github.com/Petabytz/AWS-Projects/blob/master/Windows%20Server%202012%20Migration%20with%20Domain%20and%20Active%20directory%20On%20AWS/1_63cOQQ8B04F_LBy1v0Wl4Q.png)
9. Next click Trust relationships and Copy Json code.
{
“Version”: “2012–10–17”,
“Statement”: [
{
“Sid”: “”,
“Effect”: “Allow”,
“Principal”: {
“Service”: “vmie.amazonaws.com”
},
“Action”: “sts:AssumeRole”,
“Condition”: {
“StringEquals”: {
“sts:ExternalId”: “vmimport”
}
}
}
]
}
10. and save Rolle
11. Create image for EC2 using S3 use this command
aws ec2 import-image — cli-input-json “{\”Description\”:\”<Image name>\”,\”DiskContainers\”:[{\”Description\”:\”<Image name>\”,\”UserBucket\”
:{\”S3Bucket\”:\”<Bucket name>\”,\”S3Key\”:\”<Key name>\”}}]}”
![](https://github.com/Petabytz/AWS-Projects/blob/master/Windows%20Server%202012%20Migration%20with%20Domain%20and%20Active%20directory%20On%20AWS/1_GZLANFBtFu6AjVf6FTsLzQ.png)
12. For check status of Image, conversion Used AWS CLI command
aws — output json ec2 describe-import-image-tasks — import-task-ids import
-ami-<ID>
![](https://github.com/Petabytz/AWS-Projects/blob/master/Windows%20Server%202012%20Migration%20with%20Domain%20and%20Active%20directory%20On%20AWS/1_yBEWFQCiBC4tekfXF5ecjA.png)
13. After conversion, AMI go to S3 creation region and launched EC2 instance using the snapshot of AMI or used MY AMI Tab of LaunchInstanceWizard
