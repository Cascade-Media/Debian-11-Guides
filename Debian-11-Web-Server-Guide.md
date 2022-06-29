# Debian 11 Web Server Guide
Packages, setup and management

## Contents

- [Debian 11 Web Server Guide](#debian-11-web-server-guide)
- [Covering Packages, setup and management](#covering-packages-setup-and-management)
  - [About](#about)
  - [Setup Considerations](#setup-considerations)
  - [Packages, installation & setup](#packages-installation--setup)

## About
This guide aims to assist with setting up and managing a web server running a range of powerful and complex packages.  
The guide will contain limited information on using and debugging the packages that will be installed or mentioned.  

The current list of packages being used in this guide can be found below.
- [✔️]	Unattended Upgrades
- [✔️]	OpenSSH Server
- [✔️]	UFW
- [✔️]	GUFW
- [✔️]	PHP-FPM
- [✔️]	Composer
- [✔️]	MySQL
- [❌]	PostgreSQL
- [❌]	Gitlab CE
- [❌]	NPM
- [❌]	Node JS
- [❌]	MongoDB

## Setup Considerations
Before continuing, it is expected that you have prior experience of Linux in understanding how to get around Debian, install and configure packages locally or through SSH, setup user permissions, basic security and network management.  

### Network
This guide will be making use of a static IP address to manage the network, if you have a dynamic IP address “an address that changes frequently”, you may have to make use of DNS services.  
It is untested here, but a service such as No-IP may be able to assist as a static IP vendor for your Dynamic IP address.  

### SFTP
Secure File Transfer Protocol Clients (SFTP) such as WinSCP will be required to perform SFTP actions and to generate PuTTY SSH Keys that will be used for accessing the web server.  

## Packages, installation & setup
### UnattendedUpgrades
See: [Debian UnattendedUpgrades](https://wiki.debian.org/UnattendedUpgrades#:~:text=The%20purpose%20of%20unattended%2Dupgrades,send%20you%20emails%20about%20updates.)
### About
Allows the system to update security packages (and other) updates automatically.  

Installation
```
$ sudo apt-get install unattended-upgrades apt-listchanges
```

OpenSSH Server
OpenSSH is required in order for the user to be able to remotely access the server through secure shell.  
See: LinuxHint: Enable SSH Server on Debian.  

### Installation & Setup
#### Install open ssh server
```
$ sudo apt-get install openssh-server
```

#### Enable the ssh server 
```
$ sudo systemctl enable ssh
```

#### Start SSH
```
$ sudo systemctl start ssh
```

#### Check SSH status
```
$ sudo systemctl status ssh
```

#### Security: disable Root SSH access
See: [MediaTemple: How do I disable SSH login for the root user?](https://mediatemple.net/community/products/dv/204643810/how-do-i-disable-ssh-login-for-the-root-user#:~:text=Verify%20that%20you%20can%20su,root%20with%20the%20admin%20user.&text=To%20disable%20root%20SSH%20login,with%20your%20favorite%20text%20editor.&text=Ensure%20that%20you%20are%20logged,yourself%20out%20of%20the%20server.)
The root user is a primary target when it comes to SSH access related attacks as it has the master authority over the system, It is advised that you disable Root access over SSH for this very reason in the event that an SSH private key is stolen and the account becomes compromised.  

To disable SSH for root you will need to open the ssdh_config file.  
Location: /etc/ssh/ssh_config

You are free to modify the file with any method available.  
Once open you should modify the following.  

#### From
```
#PermitRootLogin prohibit-password
```

#### To
```
PermitRootLogin no
```

Once done, save the file and close.  
Root will now no longer be able to make use of SSH.  
Once Done we should create or modify an existing user.  
See: Secure Shell (SSH) & Secure File Transfer Protocol (SFTP) setup.  

## UFW
UFW also known as Uncomplicated Firewall is a system tool that allows you to open, block and manage UDP and TCP ports within your system, restricting what tools can be accessed over a network.  
Both UFW and Gufw are excellent packages to enhance the security of your system.  

Guides on these packages can be found below.  
### Documentation
-	[Ubuntu Documentation - UFW - Uncomplicated Firewall](https://help.ubuntu.com/community/UFW)
-	[Ubuntu Documentation - Gufw](https://help.ubuntu.com/community/Gufw)
-	[Debian Documentation - Uncomplicated Firewall (ufw)](https://wiki.debian.org/Uncomplicated%20Firewall%20%28ufw%29)
-	[Ostechnix - How To Setup Firewall With Gufw On Linux Desktop](https://ostechnix.com/how-to-setup-firewall-with-gufw-on-linux-desktop/)

### Unblocking Ports
For applications you will be allowing through, you MUST allow the ports, otherwise services such as SFTP, SSH or SCP may not work.  

#### Standard Ports
- TPC : 80 / 8080 : HTTP
- TCP : 443 : HTTPS
- TCP : 22 : SSH & SCP

## LetsEncrypt
LetsEncrypt allows you to generate SSL certificates for Domains that may require a HTTPS connection.  

This will be relevant for setting up websites, API’s and applications such as Gitlab and phpMyAdmin.  
### Installation
```
$ sudo apt install certbot python3-certbot-apache
```
### Usage
Apache will be making use of cerbot to create and renew certificates.  
To see the process, see: [Register SSL Certificate](#registerSslCertificate)  

## Network Config
This part of the guide is to help setup and access your server and its applications.  
You may require a static IP address to setup and access the server during configuration.  

### Firewall Config
Should set UFW / GUFW to allow for the relevant ports of the applications you are using.  
Settings for the UFW should correspond with the port number and port protocol.  

### DNS setup
Domain Name Server (DNS) will be required to be setup by your domain provider, most providers allow you to add DNS records.  

With an IPV4 IP address you will need to create an “A” Type record, for IPV6 IP addresses you must use an “AAAA” Type record.    
### Port Forwarding and Configuration
#### Opening Ports
Port forwarding is done by opening ports on your router, depending on your router there may be different ways to achieve this, it is recommended to read your routers guide / manual to assist with this process.  
#### Apache
Allow Apache to Listen for Ports.  
By Default Ports.conf will be listening for port 80 and port 443.  

If you operate other devices that use port 80 to renew their SSL certificates such as NAS, you can renew certificates through a process known as DNS-01.  

Port 80 allows certificates to be registered on a challenge known as HTTP-01.  

##### Open ports.conf
$ sudo nano /etc/apache2/ports.conf    

##### Ports config
Apply the appropriate port listening settings to port.conf  

![image](https://user-images.githubusercontent.com/50721672/176326968-ca39ef67-14d3-4ebb-bb06-8de79f2ae25a.png)
Save: CTRL + S  
Exit: CTRL + X  

#### SSH
SSH uses Port 22 by default, but can be altered to use any other available port.  
With OpenSSH running, you should be able to SSH into your server through a remote terminal with the use of an SSH private key as long as the server has the corresponding public key.  

on Windows using PowerShell, you should be able to access your server with the following command.  
```
ssh -p 22 -i ~/.ssh/privateKey username@serverIPaddress
```

### PHP-FPM
#### About
PHP-FPM / PHP FastCGI Process Manager was chosen over mod_php as mod_php can lock out processes and disrupt performance of websites running, there are additional security benefits that PHP-FPM offer as well as optimal performance, allowing for of up to 300% - 350% faster loading times over mod_php.  
#### Installation & Setup
See: [LinuxCapable - Install/Upgrade PHP 8.1 on Debian 11 Bullseye](https://www.linuxcapable.com/how-to-install-php-8-1-on-debian-11-bullseye/)  
#### Update Debian System
```
$ sudo apt update && sudo apt upgrade -y
```
#### Install required Dependencies
```
$ sudo apt-get install ca-certificates apt-transport-https software-properties-common wget curl lsb-release -y
```
#### Import Sury PHP Repository
```
$ curl -sSL https://packages.sury.org/php/README.txt | sudo bash -x
$ sudo apt update
$ sudo apt upgrade
```
#### Install Apache2 with PHP 8.1
```
$ sudo apt install php8.1 libapache2-mod-php8.1
$ sudo systemctl restart apache2
```
#### Upgrade Apache2 with PHP8.1-FPM & FastCGI
```
$ sudo apt install php8.1-fpm libapache2-mod-fcgid
```
#### Restart Apache
```
$ sudo systemctl restart apache2
```
#### Check Apache & PHP-FPM status
```
$ sudo systemctl status apache2
$ sudo systemctl status php8.1-fpm
```

#### Check PHP version & extensions installed
```
$ php –version
$ php -m
```

You may notice cURL is missing from the extensions list.  
To resolve this, we can install curl for php, additionally if you’d like you can add MySQL for php to have access to a relational database.  

Install the PHP cURL & MySQL Module and check extensions available.  
See: [Supported Modules](https://help.superhosting.bg/en/php-modules.html)
```
$ sudo apt install php-curl php-mysql
$ php -m
```

## Composer
### About
Composer is a dependency manager for PHP, allowing you to install packages from Packagist.  
It  requires PHP to be installed on the intended system.  

See: [Packagist](https://packagist.org/)
See: [Download Composer](https://getcomposer.org/download/)
### Installation & Setup
#### Install Composer
```
$ php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
$ php -r "if (hash_file('sha384', 'composer-setup.php') === '906a84df04cea2aa72f40b5f787e49f22d4c2f19492ac310e8cba5b96ac8b64115ac402c8cd292b8a03482574915d1a8') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
$ php composer-setup.php
$ php -r "unlink('composer-setup.php');"
```
#### Move composer.phar to allow composer to function in all directories
```
$ sudo mv composer.phar /usr/local/bin/composer
```
#### Check Composer is functioning
```
CD to any directory
$ composer about
```

#### Notes
Once composer is installed and running with the configuration above, it should be usable on any system account that can access /usr/local/bin/composer  
