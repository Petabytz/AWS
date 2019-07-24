# Jenkins_installation
## Installation  Guide for Jenkins on Redhat Linux 8 (AWS EC2)

<p> 
  Jenkins is a self-contained, open source automation server which can be used to automate all sorts of tasks related to building, testing, and delivering or deploying software.
<br>
 Features :- 
<ul>
  
<li> Can be installed through native system packages, Docker, or even run standalone by any machine with a Java Runtime Environment (JRE) installed.
 <li> Highly extensible product whose functionality can be extended through the installation of plugins.
 <li> Used to automate the non-human part of the software development process, with continuous integration and facilitating technical aspects of continuous delivery.
  <li> Supports version control tools, including AccuRev, CVS, Subversion, Git, Mercurial, Perforce, TD/OMS, ClearCase and RTC, and can execute Apache Ant, Apache Maven and sbt based projects as well as arbitrary shell scripts and Windows batch commands
 </ul>
</p>
 <h2>
  AWS security group configuration for Jenkins : - 
 </h2>
 
 
 |Port | Protocol/Service |
 |-----|------------------|
 |22   |             SSh  |
 |8080 |   Custom TCP     |
 
 
<img alt="Security Group" src="https://gitresource.s3.us-east-2.amazonaws.com/Jenkins_installation/Screenshot+from+2019-07-24+18-11-18.png">
 
### Step 0
<p>
  Login into your instance using ssh and perform an quick update.
  
  ```bash
  ssh -i /path/to/key/file hostname@ip_address
  sudo yum update 
  ```
</p>
  
  ### Step 1
<p> 
  Install dependency for Jenkins installation : -

  ```bash
 sudo yum install java
  ```
  </p>
  Add the Jenkins repository from the Jenkins website to the package manager first so that we can install it directly from our package manager.
  
  ```bash
sudo wget -O /etc/yum.repos.d/jenkins.repo http://pkg.jenkins-ci.org/redhat/jenkins.repo
sudo rpm --import https://jenkins-ci.org/redhat/jenkins-ci.org.key
```
  <br>
  Now update the packages and install Jenkins using the command : -
  
  ```bash
  sudo yum update
  sudo yum install jenkins
  ```
  <p>
  start and enable the jenkins service using systemctl :-
  
  ```bash
  sudo systemctl enable jenkins
  sudo systemctl start jenkins
  ```
  </p>
  
  Now,open your browser and hit your IP address with port 8080,(example : 35.175.231.168:8080 ),it should open a web page asking for One Time Password to unlock Jenkings.
  
 <img src="https://gitresource.s3.us-east-2.amazonaws.com/Jenkins_installation/Screenshot+from+2019-07-24+18-22-45.png">
 
 Login to the server and read the output of this file using cat command and paste it in the window.After successful authentication it will prompt you to install plugins ,choose "Install suggested plugins" .
 <img src="https://gitresource.s3.us-east-2.amazonaws.com/Jenkins_installation/Screenshot+from+2019-07-24+18-23-16.png"> <br>
 <img src="https://gitresource.s3.us-east-2.amazonaws.com/Jenkins_installation/Screenshot+from+2019-07-24+18-23-51.png"> <br>
 
 <p>
  After the plugin installation it will ask you to set admin user name and password ,put values of your choice and move forward.
  <img src="https://gitresource.s3.us-east-2.amazonaws.com/Jenkins_installation/Screenshot+from+2019-07-24+18-25-06.png">
  
  
  
  </p>

<br> <br>
Next it will ask for Jenkins URL ,match it with the IP of your instance and move ahead you will see the welcome page fo Jenkins.

<br><br>
<img src="https://gitresource.s3.us-east-2.amazonaws.com/Jenkins_installation/Screenshot+from+2019-07-24+18-26-18.png">
<img src="https://gitresource.s3.us-east-2.amazonaws.com/Jenkins_installation/Screenshot+from+2019-07-24+18-26-28.png">
