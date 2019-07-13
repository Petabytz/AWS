# AWS-LAMP-Project
<h1> LAMAP Web Server Installation Manual with Wordpress </h1>

L-Linux <br>
A-Apache <br>
M-MariaDB<br>
P-PHP <br>
<hr>
<h2> Discription </h2>
<p>
 This show the installation process of wordpress over LAMP server on Ubuntu.
 </p>
    
<h2> Installation </h2>
<h3>Apache Installation </h3>
<p> The Apache HTTP Server Project is an effort to develop and maintain an open-source HTTP server for modern operating systems including UNIX and Windows.It provides a secure, efficient and extensible server that provides HTTP services in sync with the current HTTP standards.</p>

|Utility  | Value |
|---------|:---------|
|Package  | apache2 |
|Service	| apache2	|
|Port			|	80,443	|


#### Install apache using apt
```bash
$ sudo apt-get install apache2
 ```
<img src="https://github.com/Petabytz/AWS-Projects/blob/master.d/Screenshot%20from%202019-07-13%2011-43-34.png">

#### Enable and start apache2 service so that the service get started when the server reboots.
```bash
$ sudo systemctl enable apache2.service
$ sudo systemctl start apache2.service
```
<img src="https://github.com/Petabytz/AWS-Projects/blob/master.d/Screenshot%20from%202019-07-13%2013-12-18.png">

### Open the port 80 and 443 and reload firewall so that the server can listen to remote client
```bash
$ sudo ufw allow 80
$ sudo ufw allow 443
$ sudo reload
```

#### Install MariaDB server

|Utility  | Value |
|---------|:---------|
|Package  | mariadb-server |
|Service	| mariadb	|
|Port			|	3306	|

#### Enable and start mariadb service so that the service get started by default when the server reboots.
```bash
$ sudo systemctl enable mariadb.service
$ sudo systemctl start mariadb.service
