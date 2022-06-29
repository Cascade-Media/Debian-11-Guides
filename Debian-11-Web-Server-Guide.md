# Debian 11 Web Server Guide
Packages, setup and management

## Contents
- [Debian 11 Web Server Guide](#debian-11-web-server-guide)
  * [About](#about)
  * [Setup Considerations](#setup-considerations)
    + [Network](#network)
    + [SFTP](#sftp)
  * [Packages, installation & setup](#packages--installation---setup)
    + [UnattendedUpgrades](#unattendedupgrades)
    + [About](#about-1)
    + [Installation & Setup](#installation---setup)
      - [Install open ssh server](#install-open-ssh-server)
      - [Enable the ssh server](#enable-the-ssh-server)
      - [Start SSH](#start-ssh)
      - [Check SSH status](#check-ssh-status)
      - [Security: disable Root SSH access](#security--disable-root-ssh-access)
      - [From](#from)
      - [To](#to)
  * [UFW](#ufw)
    + [Documentation](#documentation)
    + [Unblocking Ports](#unblocking-ports)
      - [Standard Ports](#standard-ports)
  * [LetsEncrypt](#letsencrypt)
    + [Installation](#installation)
    + [Usage](#usage)
  * [Network Config](#network-config)
    + [Firewall Config](#firewall-config)
    + [DNS setup](#dns-setup)
    + [Port Forwarding and Configuration](#port-forwarding-and-configuration)
      - [Opening Ports](#opening-ports)
      - [Apache](#apache)
        * [Open ports.conf](#open-portsconf)
        * [Ports config](#ports-config)
      - [SSH](#ssh)
    + [PHP-FPM](#php-fpm)
      - [About](#about-2)
      - [Installation & Setup](#installation---setup-1)
      - [Update Debian System](#update-debian-system)
      - [Install required Dependencies](#install-required-dependencies)
      - [Import Sury PHP Repository](#import-sury-php-repository)
      - [Install Apache2 with PHP 8.1](#install-apache2-with-php-81)
      - [Upgrade Apache2 with PHP8.1-FPM & FastCGI](#upgrade-apache2-with-php81-fpm---fastcgi)
      - [Restart Apache](#restart-apache)
      - [Check Apache & PHP-FPM status](#check-apache---php-fpm-status)
      - [Check PHP version & extensions installed](#check-php-version---extensions-installed)
  * [Composer](#composer)
    + [About](#about-3)
    + [Installation & Setup](#installation---setup-2)
      - [Install Composer](#install-composer)
      - [Move composer.phar to allow composer to function in all directories](#move-composerphar-to-allow-composer-to-function-in-all-directories)
      - [Check Composer is functioning](#check-composer-is-functioning)
      - [Notes](#notes)
  * [Apache Servers Config](#apache-servers-config)
    + [Memory Limit](#memory-limit)
      - [Increasing your memory limit](#increasing-your-memory-limit)
      - [Locate the line](#locate-the-line)
      - [Update line and save file](#update-line-and-save-file)
      - [Restart PHP](#restart-php)
      - [Testing in PHP](#testing-in-php)
    + [Required Modules](#required-modules)
    + [Debug Notes](#debug-notes)
      - [Slow Curl Requests](#slow-curl-requests)
      - [Open the Resolver configuration file](#open-the-resolver-configuration-file)
      - [Add the following to a newline](#add-the-following-to-a-newline)
  * [Apache Site Settings](#apache-site-settings)
    + [Creating Directories and setting Group permissions](#creating-directories-and-setting-group-permissions)
      - [Creating directories for each user account](#creating-directories-for-each-user-account)
      - [Creating directories for each domain and subdomain](#creating-directories-for-each-domain-and-subdomain)
      - [Assign user permissions to the directories](#assign-user-permissions-to-the-directories)
    + [Creating your own Server settings](#creating-your-own-server-settings)
      - [Create the Configuration file](#create-the-configuration-file)
        * [HTTP Config](#http-config)
          + [HTTP Template](#http-template)
        * [HTTPS Config](#https-config)
          + [HTTPs Template](#https-template)
    + [Enable Sites](#enable-sites)
        * [Enable Sites](#enable-sites-1)
        * [Disable Sites](#disable-sites)
        * [Reload Apache2](#reload-apache2)
        * [Restart Apache2](#restart-apache2)
        * [Status of Apache2](#status-of-apache2)
      - [Notes](#notes-1)
    + [Registering SSL Certificates](#registering-ssl-certificates)
      - [Register a certificate](#register-a-certificate)
      - [Restart Apache](#restart-apache-1)
      - [Check for SSL certificate is valid](#check-for-ssl-certificate-is-valid)
      - [Debugging](#debugging)
    + [Hiding your Apache Version and Operating System from Error Pages](#hiding-your-apache-version-and-operating-system-from-error-pages)
      - [Open security.conf](#open-securityconf)
      - [Make Changes](#make-changes)
      - [Save and Restart server](#save-and-restart-server)

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

## PHP-FPM
### About
PHP-FPM / PHP FastCGI Process Manager was chosen over mod_php as mod_php can lock out processes and disrupt performance of websites running, there are additional security benefits that PHP-FPM offer as well as optimal performance, allowing for of up to 300% - 350% faster loading times over mod_php.  
### Installation & Setup
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
CD to any directory  
```
$ composer about
```

#### Notes
Once composer is installed and running with the configuration above, it should be usable on any system account that can access /usr/local/bin/composer  

## Apache Servers Config
Once your Apache server is up and running with PHP-FPM, you can locate your root folder for php projects under /var/www/html/  

an index.php file will be located within this directory and is accessible in your browser through localhost or 127.0.0.1 and should output the Apache2 Debian default page which contains basic configuration guidance.  

### Memory Limit
While PHP doesn’t require much memory to operate, a few frameworks and applications may require a higher memory allocation, by default php makes use of 128Mb.
For projects running Symfony or Laravel, this might be required as in some cases developer tool panels may not appear.  

#### Increasing your memory limit
```
$ sudo nano /etc/php/8.1/fpm/php.ini
```
#### Locate the line
```
memory_limit = 128M
```
![image](https://user-images.githubusercontent.com/50721672/176327952-8fa5b8bf-1db5-426a-96c5-261a94fd5103.png)
#### Update line and save file
```
memory_limit 256M
```
#### Restart PHP
```
$ sudo systemctl restart php8.1-fpm
```
#### Testing in PHP
```php
$memory_limit = ini_get('memory_limit');
echo $memory_limit;
```

### Required Modules
Required for installation of software in this guide.  
```
$ sudo a2enmod headers
$ sudo a2enmod rewrite
$ sudo a2enmod ssl
$ sudo a2enmod proxy
$ sudo a2enmod proxy_http
$ sudo systemctl restart apache2
```

### Debug Notes
#### Slow Curl Requests
If at any point you are using packages such as Symfony’s HttpClient, Guzzle or Curl to make HTTP requests to an external API, there’s a possibility you may get an issue with requests taking a long time to execute.  

If this is the case, it could be down to a conflict between ipv4 (A) and ipv6(AAAA) trying to make use of the same sockets and port while resolving the DNS.  

See Sources: 
-	[Slow DNS on Linux fix (ipv6/ipv4 conflict)](https://aarvik.dk/disable-ipv6/)
-	[resolv.conf(5) — Linux manual page](https://man7.org/linux/man-pages/man5/resolv.conf.5.html)
To resolve this, we can change some settings in the resolver configuration file.

#### Open the Resolver configuration file
```
$ sudo nano /etc/resolv.conf
```

#### Add the following to a newline
```
options single-request-reopen
```

## Apache Site Settings
Each website should have its own virtual host configuration in order to load on a specific port, domain and with a security certificate in the form of SSL / TLS.  
Additionally each website can have its own directory and file permissions in order to limit or prevent access to certain files in projects, these files may include .env which often contain sensitive information such as private, public and API keys.  

Under the directory /etc/apache2/sites-available/ are likely two files “000-default.con” and “default-ssl.conf”  

These are necessary to load the apache2 Debian Default Page shown in the above image.  
### Creating Directories and setting Group permissions
When creating new directories for your projects, you will likely need to setup directory ownership and read, write, execute permissions.  

#### Creating directories for each user account
```
$ sudo mkdir /var/www/html/username
```
#### Creating directories for each domain and subdomain
```
$ sudo mkdir /var/www/html/username/example.com
$ sudo mkdir /var/www/html/username/example.com/www.example.com
```

#### Assign user permissions to the directories
```
$ sudo chown -R username:www-data /var/www/html/username
```

### Creating your own Server settings
#### Create the Configuration file
This command will allow you to create the file(‘s) necessary to manage your domain, above are the provided HTTP and HTTPS settings, you will need to make a config file for each of these for a single domain in order to secure the connection.   

##### HTTP Config
```
$ sudo nano /etc/apache2/sites-available/www.example.com-http.conf
```
Apply the HTTP Template to the file and adjust for your requirements. 
###### HTTP Template
```conf
# Organised File Naming Examples
# username_domainName_subdomain_http
# username_domainName_subdomain_https

# Virtual Host can operate on Port 80 or Port 8080 as normal.
# However, Port 80 is required to register SSL certificates

# Virtual Host ports can be almost any number, but must use port 
# at the end of the domain name e.g. example.com:12080 
<VirtualHost *:80>
    ServerName www.example.com

    # Server Config
    Redirect permanent / https://www.example.com

    RewriteEngine on
    RewriteCond %{SERVER_NAME} =www.example.com
    RewriteRule ^ https://%{SERVER_NAME}%{REQUEST_URI} [END,NE,R=permanent]
</VirtualHost>
```

##### HTTPS Config
Apply the HTTPs Template to the file and adjust for your requirements. 
```
$ sudo nano /etc/apache2/sites-available/www.example.com-https.conf
```
###### HTTPs Template
```conf
<IfModule mod_ssl.c>
    <VirtualHost *:443>
        ServerName www.example.com
        ServerAdmin admin@example.com

        # Server Config     
        DocumentRoot /var/www/html/.username/example.com/www.example.com

        <Directory /var/www/html/.username/example.com/www.example.com>
            AllowOverride All    
            Order Allow,Deny        
            Allow from All           
        </Directory>
    
        <FilesMatch /(/?|/.*)>
            Order allow,deny
        </FilesMatch>

        <FilesMatch "\.(cgi|shtml|phtml|php)$">
            SSLOptions +stdEnvVars
        </FilesMatch>   
        
        # Logging
        ErrorLog ${APACHE_LOG_DIR}/example.com-error.log
        CustomLog ${APACHE_LOG_DIR}/example.com-access.log combined
        
        # Resolve Older Browser issues      
        BrowserMatch "MSIE [2-6]"nokeepalive-ssl-unclean-shutdown downgrade-1.0 force-response-1.0
        BrowserMatch "MSIE [17-9]" ssl-unclean-shutdown

        # SSL
    </VirtualHost>
</IfModule>
```

### Enable Sites
By Default, sites are disabled by Apache, you MUST enable your sites and reload Apache before the sites can be located on the server when visiting the designated domain name.  

##### Enable Sites
```
$ sudo a2ensite www.example.com-http.conf
$ sudo a2ensite www.example.com-https.conf
```
##### Disable Sites
Not required, but useful to know if you wish to disable an active site, possibly for debugging.  
```
$ sudo a2dissite filename-https.conf
```
##### Reload Apache2
```
$ sudo systemctl reload apache2
```
##### Restart Apache2
```
$ sudo systemctl restart apache2
```
##### Status of Apache2
Checks if the server is active or failed to activate.  
```
$ sudo systemctl status apache2
```
#### Notes
Sites enabled won’t get your website online immediately, you will need to perform some network configurations and port forwarding on both your Domain Name Server (DNS) and on your own router.  

### Registering SSL Certificates
In order for your website to be secure, it will require an SSL certificate on the HTTPS configuration.  
This is possible with the help of OpenSSH’s LetsEncrypt through port 80 or Port 443, which are required in order to request certificates.  
See: [Lets Encrypt - Challenge Types](https://letsencrypt.org/docs/challenge-types/)

#### Register a certificate
```
$ sudo certbot –apache
```
- Select your site by ID available.  
- Select the most valid option available regarding your certificate.  

#### Restart Apache
```
$ sudo systemctl restart apache2
```

#### Check for SSL certificate is valid
We can do this by checking the padlock on the target https domain in a browser such as Chrome, Edge, Firefox, etc..

![image](https://user-images.githubusercontent.com/50721672/176329145-32229a26-4aa1-4293-9716-833f94e48976.png)

#### Debugging
-	If the domain is showing invalid, you may have misconfigured some settings on Apache, your DNS host.
-	The DNS may not be validated for your IP address yet.
-	The browser may have cached the active sessions certificate “restart browser or try another browser”
-	You may be viewing the site over http instead of https, check the URL

### Hiding your Apache Version and Operating System from Error Pages
Hiding your Operating System and Apache server version is a good measure to take in order to help secure your server.  

Obscurity is not a security measure, but it will make any potential attacks on the server harder to undertake.  

When an error page is shown such as a 404, page not found, we get the following error message showcasing what Apache version, Operating System and Port the website is running on.  

![image](https://user-images.githubusercontent.com/50721672/176329212-3fa753b1-616d-45e0-b415-4a4c2787561e.png)

#### Open security.conf
```
$ sudo nano /etc/apache2/conf-enabled/security.conf
```

#### Make Changes 
```
ServerTokens Prod  
ServerSignature Off
```  

#### Save and Restart server
```
systemctl restart apache2  
```

After making the changes, we now get vastly less information regarding the server setup.

![image](https://user-images.githubusercontent.com/50721672/176329275-d3625f41-c4f4-4b82-9876-1a18e8e7d49c.png)


## MariaDB
### About
MariaDB is a powerful, well-known and widely used open-source MySQL relational database.  
It has all the core features to support major web applications such as WordPress, Drupal, Symfony, Laravel and other Dynamic frameworks and content management systems.  
### Installation & Setup
See: [LinuxCapable - How to Install or Upgrade MariaDB 10.8 on Debian 11 Bullseye](https://www.linuxcapable.com/install-upgrade-mariadb-10-8-on-debian-11-bullseye/)
#### Update Debian System
```
$ sudo apt update && sudo apt upgrade -y
```
#### Install Required Dependencies
```
$ sudo apt-get install curl software-properties-common dirmngr ca-certificates apt-transport-https -y
```
#### Install curl (If not already installed)
Curl is required for the installation of the next step. 
``` 
$ sudo apt install curl -y
$ sudo apt update
```
#### Import MariaDB 10.8 GPG Keys & Repository
```
$ curl -LsS https://downloads.mariadb.com/MariaDB/mariadb_repo_setup | sudo bash -s -- --mariadb-server-version=10.8
$ sudo apt update
```
#### Install or Upgrade MariaDB
```
$ sudo apt install mariadb-server mariadb-client
```
#### Verify MariaDB version installed and check status
```
$ mariadb –version
$ systemctl status mariadb
```
#### Additional Commands
```
Start MariaDB : $ sudo systemctl start mariadb
Stop MariaDB : $ sudo systemctl stop mariadb
Enable on Startup : $ sudo systemctl enable mariadb
Restart MariaDB : $ sudo systemctl restart mariadb
```

#### Run MariaDB Security Script
```
$ sudo mysql_secure_installation
```
```
•	Switch to unix_socket authentication [Y/n] : Y
•	Change the root password? [Y/n] : Y
•	Remove anonymous users? [Y/n] : Y
•	Disallow root login remotely? [Y/n] Y
•	Remove test database and access to it? [Y/n] : Y
•	Reload privilege tables now? [Y/n] : Y
```

#### Upgrading MariaDB
```
$ sudo mariadb-upgrade
```

#### Uninstall MariaDB
```
$ sudo apt autoremove mariadb-server mariadb-client --purge -y
```

#### Remove bash script repository
```
$ sudo rm /etc/apt/sources.list.d/mariadb.list
$ sudo apt update
```

## PHPMyAdmin
PhpMyAdmin is a graphical user interface tool(GUI) that works with MySQL MariaDB, this tool allows you to login to a webserver and manage the database through the GUI. 
### Important Notice
PHP 8.1.4 supports PhpMyAdmin 5.1.3, however you may encounter issues exporting or backing up your database, for this reason it is recommended that you avoid installing PhpMyAdmin 5.1.3 on PHP 8.1.4 or later as this is a known issue. 
see: [Export is not working on v5.1.3 (PHP version: 8.1.4) #17477](https://github.com/phpmyadmin/phpmyadmin/issues/17477) 
### Requirements
MariaDB and Apache **MUST** be installed, before continuing.  

Apache: [Installation & Setup](#install-apache2-with-php-81)  
MariaDB: [Installation & Setup](#MariaDB)    
In this guide, Apache was installed with the PHP-FPM configuration, you may install a different variant at your own discretion.    

#### mbstring Apache Module
This module is required for phpMyAdmin
```
$ sudo apt-get install php-mbstring
```

### Installation & Setup
#### Install PHP extensions required for PHPMyAdmin
See: [How To Install phpMyAdmin on Debian 11/Debian 10](https://computingforgeeks.com/install-phpmyadmin-with-apache-on-debian/)
```
$ sudo apt -y update
$ sudo apt -y install wget php php-cgi php-pear php-mbstring libapache2-mod-php php-common php-phpseclib php-mysql php8.1-mysql
```

#### Download the latest version of phpMyAdmin
```
$ cd ~
$ DATA="$(wget https://www.phpmyadmin.net/home_page/version.txt -q -O-)" 
URL="$(echo $DATA | cut -d ' ' -f 3)" 
VERSION="$(echo $DATA | cut -d ' ' -f 1)" 
wget https://files.phpmyadmin.net/phpMyAdmin/${VERSION}/phpMyAdmin-${VERSION}-all-languages.tar.gz
```
#### Extract downloaded archive
```
$ tar xvf phpMyAdmin-${VERSION}-all-languages.tar.gz
```
#### Move the default phpMyAdmin installation folder and rename folder
```
$ sudo mv phpMyAdmin-*/ /var/www/html/services
```
#### Depending on the version installed of your phpmyadmin, you may need to type in your folder name manually in the command below.
```
$ cd /var/www/html/services/
$ sudo mv phpMyAdmin-5.2.0-all-languages phpmyadmin
```
#### Create temporary files folder for phpMyAdmin
```
$ cd /var/www/html/services/phpmyadmin
$ sudo mkdir temp
```
#### Give ownership of phpMyAdmin folder to www-data user group
```
$ sudo chown -R www-data:www-data /var/www/html/services/phpmyadmin
Change permissions and ownership of Temp directory
$ sudo chmod 755 /var/www/html/services/phpmyadmin/temp/
$ sudo chown www-data:www-data /var/www/html/services/phpmyadmin/temp/
```
#### Make a config file using a copy of the config. Sample
```
$ sudo cp /var/www/html/services/phpmyadmin/config.sample.inc.php  /var/www/html/services/phpmyadmin/config.inc.php
```
#### Secure phpMyAdmin with secret passphrase
[Generate a secret passphrase](https://www.lastpass.com/features/password-generator) and use it in the blowfish secret config.  
a very strong password method is advised and MUST BE 32 characters long.  

#### Open phpMyAdmin’s configuration file
```
$ sudo nano /var/www/html/services/phpmyadmin/config.inc.php
```

#### Enter or modify the following
```php
# Modify
/**
 * This is needed for cookie based authentication to encrypt password in
 * cookie. Needs to be 32 chars long.
 */
$cfg['blowfish_secret'] = 'YOUR SECRET PASSPHRASE'; /* YOU MUST FILL IN THIS FOR COOKIE AUTH! */

# Add
/** 
 * Temporary Directory
 */
$cfg['TempDir'] = '/var/www/html/services/phpmyadmin/temp';
```
#### To Save the file in Nano, then close
Save: CTRL + O
Exit: CRTL + X

## Accessing PHPMyAdmin
Once Apache has been restarted, you should be able to access your phpMyAdmin panel through localhost/phpmyadmin or 127.0.0.1/phpmyadmin  
![image](https://user-images.githubusercontent.com/50721672/176330762-1cbcb34a-d63f-499d-808a-ff977223dcbd.png)

it is possible to setup a domain name to point to the server, this will be covered later in the guide.  
### Notes
#### Login Debugging
In the event you are unable to login, it may help to clear you application memory browser cache.  

## User Management
### Basic Setup on Linux Server
#### Give a non-root account sudo privileges
```
$ usermod -aG sudo exampleUsername
```
#### Change ownership of users own folder from root to self
```
$ sudo chown -R exampleUsername ~/
```

#### Give file write permissions to users SSH folder
```
$ sudo chmod -R 755
```

#### To understand what 755 is doing with chmod.
See: [Chmod Calculator: An awesome Chmod Calculator to convert Linux file permissions between different formats](https://chmod-calculator.com/)

### Secure Shell (SSH) & Secure File Transfer Protocol (SFTP) setup
This SSH key pairing guide is between a Windows 10 local machine and Linux Debian 11 server that has been tested on the local network.  

Before continuing you MUST ensure that port 22 has been opened on your router, if you are running a virtual machine you will need to also allow port 22 access in order to access the server.  

Additionally when throwing the ssh key up to the server, the target folder MUST exist or be created beforehand, otherwise the process will not be possible.  

#### Generating an SSH Key with Windows Powershell
This method allows us to create an SSH user key for SSH terminal access into the server.  
You will need to use powershell to create and upload the key.  
##### RSA Options
```
ssh-keygen -t rsa -b 2048 -C user@domain.com
ssh-keygen -t rsa -b 4096 -C user@domain.com
```

Note: This key will be stored under your system username in a .ssh folder on Windows.  
See: C:\Users\YourUsername\.ssh

#### PuTTY
If you wish for a user to use SFTP, you may need to create a separate PuTTY public and private key pair using  PuTTYgen.  

#### Throwing SSH Public Key up to Linux server
The following command takes your public RSA encryption key and stores it on the target server for the target user using the SCP(Secure Copy) method.  

If you’re not sure what your IP address is, there are many services that can help.  
See: [myip.com](https://www.myip.com/)

#### Template SSH Login over Windows Powershell
```
scp $env:USERPROFILE/.ssh/<<DIRECTORY>>/<<FILENAME>>.pub <<SERVER USERNAME>>@<<SERVER IP>>:~/.ssh 
```
Once entering your ssh key directory and settings hit enter.  
If you are prompt to continue connecting, type YES and hit enter.  
To add the key you will be required to enter the password for the user specified in the command above.  

![image](https://user-images.githubusercontent.com/50721672/176331270-5ecd6b02-e7a9-4e79-8966-5c96684bc44b.png)

On completing file transfer, you should have a public ssh key located on your server.  
You can find it under /home/username/.ssh/
![image](https://user-images.githubusercontent.com/50721672/176331338-a97ba175-8f8b-4be0-9b82-6669f636796f.png)

### Accessing the Server through local terminal or PowerShell
From here we can attempt to SSH into the server from the local machine with use of the paired private key.  

#### Throwing SSH Key from Local System to Server
ssh -p <<SSH PORT>> -i ~/.ssh/<<PRIVATE KEY>> username@<<SERVER IP>>

You will need to modify the command above for your own server, once done and entered into a terminal, you may be prompt to enter the password you gave your ssh key, followed by your target users password.  

![image](https://user-images.githubusercontent.com/50721672/176331683-2e511b4d-0f11-4550-84eb-89db9dbc0db9.png)
