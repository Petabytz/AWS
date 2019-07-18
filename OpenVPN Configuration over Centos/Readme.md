## OpenVPN Configuration over Centos
### What do you mean by OpenVPN..?
OpenVPN. OpenVPN is an open-source commercial software that implements virtual private network (VPN) techniques to create secure point-to-point or site-to-site connections in routed or bridged configurations and remote access facilities. It uses a custom security protocol that utilizes SSL/TLS for key exchange.
### Why OpenVPN..?
OpenVPN's principal strengths include cross-platform portability across most of the known computing universe, excellent stability, scalability to hundreds or thousands of clients, relatively easy installation, and support for dynamic IP addresses and NAT.
OpenVPN provides an extensible VPN framework which has been designed to ease site-specific customization, such as providing the capability to distribute a customized installation package to clients, or supporting alternative authentication methods via OpenVPN's plugin module interface (For example the openvpn-auth-pam module allows OpenVPN to authenticate clients using any PAM authentication method - such methods may be used exclusively or combined with X509 certificate-based authentication).
OpenVPN offers a management interface which can be used to remotely control or centrally manage an OpenVPN daemon. The management interface can also be used to develop a GUI or web-based front-end application for OpenVPN.
On Windows, OpenVPN can read certificates and private keys from smart cards which support the Windows Crypto API.
OpenVPN uses an industrial strenght security model designed to protect against both passive and active attacks. OpenVPN's security model is based on using SSL/TLS for session authentication and the IPSec ESP protocol for secure tunnel transport over UDP. OpenVPN supports the X509 PKI (public key infrastructure) for session authentication, the TLS protocol for key exchange, the OpenSSL cipher-independent EVP interface for encrypting tunnel data, and the HMAC-SHA1 algorithm for authenticating tunnel data.
OpenVPN is built for portability. At the time of this writing, OpenVPN runs on Linux, Solaris, OpenBSD, FreeBSD, NetBSD, Mac OS X, and Windows (2000/XP and later versions). Because OpenVPN is written as a user-space daemon rather than a kernel module or a complex modification to the IP layer, porting efforts are dramatically simplified.
OpenVPN is easy to use. In general, a tunnel can be created and configured with a single command (and without any required configuration files). OpenVPN's documentation contain examples illustrative of its ease of use.
OpenVPN has been rigorously designed and tested to operate robustly on unreliable networks. A major design goal of OpenVPN is that it should be as responsive, in terms of both normal operations and error recovery, as the underlying IP layer that it is tunneling over. That means that if the IP layer goes down for 5 minutes, when it comes back up, tunnel traffic will immediately resume even if the outage interfered with a dynamic key exchange which was scheduled during that time.
OpenVPN has been built with a strongly modular design. All of the crypto is handled by the OpenSSL library, and all of the IP tunneling functionality is provided through the TUN/TAP virtual network driver. The benefits of this modularity can be seen, for example, in the way that OpenVPN can be dynamically linked with a new version of the OpenSSL library and immediately have access to any new functionality provided in the new release. For example, when OpenVPN is built with the latest version of OpenSSL (0.9.7), it automatically has access to new ciphers such as AES-256 (Advanced Encryption Standard with 256 bit key) and the encryption engine capability of OpenSSL that allows utilization of special-purpose hardware accelerators to optimize encryption, decryption, and authentication performance. In the same way, OpenVPN's user-space design allows straightforward porting to any OS which includes a TUN/TAP virtual network driver.
OpenVPN is fast. Running Redhat 7.2 on a Pentium II 266mhz machine, using TLS-based session authentication, the Blowfish cipher, SHA1 authentication for the tunnel data, and tunneling an FTP session with large, pre-compressed files, OpenVPN achieved a send/receive transfer rate of 1.455 megabytes per second of CPU time (combined kernel and user time).
While OpenVPN provides many options for controlling the security parameters of the VPN tunnel, it also provides options for protecting the security of the server itself, such as - chroot for restricting the part of the file system the OpenVPN daemon has access to, - user and - group for downgrading daemon privileges after initialization, and - mlock to ensure that key material and tunnel data is never paged to disk where it might later be recovered.
### Step 1 - Installing OpenVPN:
To start, we will install OpenVPN on the server. We'll also install Easy RSA, a public key infrastructure management tool which will help us set up an internal certificate authority (CA) for use with our VPN. We'll also use Easy RSA to generate our SSL key pairs later on to secure the VPN connections.
Log in to the server as the non-root sudo user, and update the package lists to make sure you have all the latest versions.
### sudo yum update -y
The Extra Packages for Enterprise Linux (EPEL) repository is an additional repository managed by the Project containing non-standard but popular packages. OpenVPN isn't available in the default CentOS repositories but it is available in EPEL, so install EPEL:
### sudo yum install epel-release -y
Then update your package lists once more:
### sudo yum update -y
Next, install OpenVPN and wget, which we will use to install Easy RSA:
### sudo yum install -y openvpn wget
Using wget, download Easy RSA.
### wget -O /tmp/easyrsa https://github.com/OpenVPN/easy-rsa-old/archive/2.3.3.tar.gz
Next, extract the compressed file with tar:
### tar xfz /tmp/easyrsa
This will create a new directory on your server called easy-rsa-old-2.3.3 Make a new subdirectory under /etc/openvpn and name it easy-rsa:
### sudo mkdir /etc/openvpn/easy-rsa
Copy the extracted Easy RSA files over to the new directory:
### sudo cp -rf easy-rsa-old-2.3.3/easy-rsa/2.0/* /etc/openvpn/easy-rsa
Then change the directory's owner to your non-root sudo user:
### sudo chown petabytz /etc/openvpn/easy-rsa/
### Step 2 - Configuring OpenVPN:
OpenVPN has several example configuration files in its documentation directory. First, copy the sample server.conf file as a starting point for your own configuration file.
### sudo cp /usr/share/doc/openvpn-2.4.4/sample/sample-config-files/server.conf /etc/openvpn
Open the new file for editing with the text editor of your choice. We'll use nano in our example, which you can download with the yum install nano command if you don't have it on your server already:
### sudo nano /etc/openvpn/server.conf
To get started, find and uncomment the line containing push "redirect-gateway def1 bypass-dhcp".  Doing this will tell your client to redirect all of its traffic through  your OpenVPN server. Be aware that enabling this functionality can  cause connectivity issues with other network services, like SSH:
![](https://github.com/Petabytz/AWS-Projects/blob/master/OpenVPN%20Configuration%20over%C2%A0Centos/Screenshot%20from%202019-07-18%2016-46-47.png)
Because your client will not be able to use the default DNS servers provided by your ISP (as its traffic will be rerouted), you need to tell it which DNS servers it can use to connect to OpenVPN.
We want OpenVPN to run with no privileges once it has started, so we need to tell it to run with a user and group of nobody. To enable this, uncomment the user nobody and group nobody lines:
![](https://github.com/Petabytz/AWS-Projects/blob/master/OpenVPN%20Configuration%20over%C2%A0Centos/Screenshot%20from%202019-07-18%2016-59-39.png)
Next, uncomment the topology subnet line. This, along with the server 10.8.0.0 255.255.255.0  line below it, configures your OpenVPN installation to function as a  subnetwork and tells the client machine which IP address it should use.  In this case, the server will become 10.8.0.1 and the first client will become 10.8.0.2:
![](https://github.com/Petabytz/AWS-Projects/blob/master/OpenVPN%20Configuration%20over%C2%A0Centos/Screenshot%20from%202019-07-18%2017-03-35.png)
It's also recommended that you add the following line to your server configuration file. This double checks that any incoming client certificates are truly coming from a client, hardening the security parameters we will establish in later steps:
![](https://github.com/Petabytz/AWS-Projects/blob/master/OpenVPN%20Configuration%20over%C2%A0Centos/Screenshot%20from%202019-07-18%2017-07-25.png)
Lastly, OpenVPN strongly recommends that users enable TLS Authentication, a cryptographic protocol that ensures secure communications over a computer network. To do this, you will need to generate a static encryption key (named in our example as myvpn.tlsauth, although you can choose any name you like). Before creating this key, comment the line in the configuration file containing tls-auth ta.key 0 by prepending it with a semicolon. Then, add tls-crypt myvpn.tlsauth to the line below it:
![](https://github.com/Petabytz/AWS-Projects/blob/master/OpenVPN%20Configuration%20over%C2%A0Centos/Screenshot%20from%202019-07-18%2017-11-22.png)
Save and exit the OpenVPN server configuration file.Then generate the static encryption key with the following command:
### sudo openvpn --genkey --secret /etc/openvpn/myvpn.tlsauth
Now that your server is configured, you can move on to setting up the SSL keys and certificates needed to securely connect to your VPN connection.
### Step 3 - Generating Keys and Certificates:
Easy RSA uses a set of scripts that come installed with the program to generate keys and certificates. In order to avoid re-configuring every time you need to generate a certificate, you can modify Easy RSA's configuration to define the default values it will use for the certificate fields, including your country, city, and preferred email address.
We'll begin our process of generating keys and certificates by creating a directory where Easy RSA will store any keys and certs you generate:
### sudo mkdir /etc/openvpn/easy-rsa/keys
The default certificate variables are set in the vars file in /etc/openvpn/easy-rsa, so open that file for editing:
### sudo nano /etc/openvpn/easy-rsa/vars
Change the values that start with export KEY_ to match your information. The ones that matter the most are:
KEY_CN: Here, enter the domain or subdomain that resolves to your server.
KEY_NAME: You should enter server here. If you enter something else, you would also have to update the configuration files that reference server.key and server.crt.

The other variables in this file that you may want to change are:
KEY_COUNTRY: For this variable, enter the two-letter abbreviation of the country of your residence.
KEY_PROVINCE: This should be the name or abbreviation of the state of your residence.
KEY_CITY: Here, enter the name of the city you live in.
KEY_ORG: This should be the name of your organization or company.
KEY_EMAIL: Enter the email address that you want to be connected to the security certificate.
KEY_OU: This should be the name of the "Organizational  Unit" to which you belong, typically either the name of your department  or team.
![](https://github.com/Petabytz/AWS-Projects/blob/master/OpenVPN%20Configuration%20over%C2%A0Centos/Screenshot%20from%202019-07-18%2017-24-27.png)
Save and close the file.
To start generating the keys and certificates, move into the easy-rsa directory and source in the new variables you set in the vars file:
### cd /etc/openvpn/easy-rsa
### source ./vars
Run Easy RSA's ### "clean-all"script to remove any keys and certificates already in the folder and generate the certificate authority.
Next, build the certificate authority with the "build-ca" script. You'll be prompted to enter values for the certificate fields, but if you set the variables in the vars file earlier, all of your options will already be set as the defaults. You can press ENTER to accept the defaults for each one.
This script generates a file called ca.key. This is the  private key used to sign your server and clients' certificates. If it is  lost, you can no longer trust any certificates from this certificate  authority, and if anyone is able to access this file they can sign new  certificates and access your VPN without your knowledge.
Next, create a key and certificate for the server using the build-key-server script:
### ./build-key-server server
The last part of creating the server keys and certificates is generating a Diffie-Hellman key exchange file. Use the build-dh script to do this:
./build-dh
This may take a few minutes to complete.
Once your server is finished generating the key exchange file, copy the server keys and certificates from thekeys directory into the openvpn directory:
### cd /etc/openvpn/easy-rsa/keys
### sudo cp dh2048.pem ca.crt server.crt server.key /etc/openvpn
Each client will also need a certificate in order for the OpenVPN server to authenticate it. These keys and certificates will be created on the server and then you will have to copy them over to your clients.It's advised that you generate separate keys and certificates for each client you intend to connect to your VPN.
Because we'll only set up one client here, we called it client, but you can change this to a more descriptive name if you'd like:
### cd /etc/openvpn/easy-rsa
### ./build-key client
Finally, copy the versioned OpenSSL configuration file, openssl-1.0.0.cnf, to a versionless name, openssl.cnf
### cp /etc/openvpn/easy-rsa/openssl-1.0.0.cnf /etc/openvpn/easy-rsa/openssl.cnf
Now that all the necessary keys and certificates have been generated for your server and client, you can move on to setting up routing between the two machines.
### Step 4 - Routing:
OpenVPN with any instructions on where to send incoming web traffic from clients. You can stipulate how the server should handle client traffic by establishing some firewall rules and routing configurations.
To allow OpenVPN through the firewall, you'll need to know what your active firewalld zone is. Find this with the following command:
### sudo firewall-cmd --get-active-zones
Output
trusted
  Interfaces: tun0
  Next, add the openvpn service to the list of services  allowed by firewalld within your active zone, and then make that setting  permanent by running the command again but with the --permanent option added:
### sudo firewall-cmd --zone=trusted --add-service openvpn
### sudo firewall-cmd --zone=trusted --add-service openvpn --permanent
You can check that the service was added correctly with the following command:
### sudo firewall-cmd --list-services --zone=trusted
Output
openvpn
Next, add a masquerade to the current runtime instance, and then add it again with the --permanent option to add the masquerade to all future instances:
### sudo firewall-cmd --add-masquerade
### sudo firewall-cmd --permanent --add-masquerade
You can check that the masquerade was added correctly with this command:
### sudo firewall-cmd --query-masquerade
Output
yes
Next, forward routing to your OpenVPN subnet. You can do this by first creating a variable (SHARK  in our example) which will represent the primary network interface used  by your server, and then using that variable to permanently add the  routing rule:
### SHARK=$(ip route get 8.8.8.8 | awk 'NR==1 {print $(NF-2)}')
sudo firewall-cmd --permanent --direct --passthrough ipv4 -t nat -A POSTROUTING -s 10.8.0.0/24 -o $SHARK -j MASQUERADE
Be sure to implement these changes to your firewall rules by reloading firewalld:
### sudo firewall-cmd --reload
Next, enable IP forwarding. This will route all web traffic from your client to your server's IP address, and your client's public IP address will effectively be hidden.
Open
### sudo nano /etc/sysctl.conf
Then add the following line at the top of the file:
![](https://github.com/Petabytz/AWS-Projects/blob/master/OpenVPN%20Configuration%20over%C2%A0Centos/Screenshot%20from%202019-07-18%2019-10-03.png)
Finally, restart the network service so the IP forwarding will take effect:
### sudo systemctl restart network.service
With the routing and firewall rules in place, we can start the OpenVPN service on the server.
### Step 5 - Starting OpenVPN:
OpenVPN is managed as a systemd service using systemctl. We  will configure OpenVPN to start up at boot so you can connect to your  VPN at any time as long as your server is running. To do this, enable  the OpenVPN server by adding it to systemctl:
### Sudo systemctl -f enable openvpn@server.service
Then start the OpenVPN service:
### sudo systemctl start openvpn@server.service
Double check that the OpenVPN service is active with the following command. You should see active (running) in the output:
### sudo systemctl status openvpn@server.service
Output
● openvpn@server.service - OpenVPN Robust And Highly Flexible Tunneling Application On server
   Loaded: loaded (/usr/lib/systemd/system/openvpn@.service; enabled; vendor preset: disabled)
   Active: **active (running)** since Wed 2018-03-14 15:20:11 EDT; 7s ago
 Main PID: 2824 (openvpn)
   Status: "Initialization Sequence Completed"
   CGroup: /system.slice/system-openvpn.slice/openvpn@server.service
           └─2824 /usr/sbin/openvpn --cd /etc/openvpn/ --config server.conf
. . .
We've now completed the server-side configuration for OpenVPN. Next, you will configure your client machine and connect to the OpenVPN server.
### Step 6 - Configuring a Client
Locate the following files on your server. If you generated multiple client keys with unique, descriptive names, then the key and certificate names will be different. In this article we used client.
### /etc/openvpn/easy-rsa/keys/ca.crt
/etc/openvpn/easy-rsa/keys/client.crt
/etc/openvpn/easy-rsa/keys/client.key
/etc/openvpn/myvpn.tlsauth
Copy these files to your client machine. You can use SFTP or your preferred method. You could even just open the files in your text editor and copy and paste the contents into new files on your client machine. Regardless of which method you use, be sure to note where you save these files.
Next, create a file called client.ovpn on your client machine. This is a configuration file for an OpenVPN client, telling it how to connect to the server:
### sudo nano client.ovpn
Then add the following lines to client.ovpn. Notice that many of these lines reflect those which we uncommented or added to the server.conf file, or were already in it by default:
![](https://github.com/Petabytz/AWS-Projects/blob/master/OpenVPN%20Configuration%20over%C2%A0Centos/Screenshot%20from%202019-07-18%2019-28-48.png)
When adding these lines, please note the following:
You'll need to change the first line to reflect the name you gave the client in your key and certificate; in our case, this is just client
You also need to update the IP address from your_server_ip to the IP address of your server; port 1194 can stay the same
Make sure the paths to your key and certificate files are correct.

Conclusion:
You should now have a fully operational virtual private network running on your OpenVPN server. You can browse the web and download content without worrying about malicious actors tracking your activity.
### ./build-key-server server

