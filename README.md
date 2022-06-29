# Debian 11 Web Server Guide
## Covering Packages, setup and management

## About
This guide aims to assist with setting up and managing a web server running a range of powerful and complex packages.

The guide will contain limited information on using and debugging the packages that will be installed or mentioned.

The current list of packages being used in this guide can be found below.
[x]	Unattended Upgrades
[x]	OpenSSH Server
[x]	UFW
[x]	GUFW
[x]	PHP-FPM
[x]	Composer
[x]	MySQL
[]	PostgreSQL
[]	Gitlab CE
[]	NPM
[]	Node JS
[]	MongoDB

##Setup Considerations
Before continuing, it is expected that you have prior experience of Linux in understanding how to get around Debian, install and configure packages locally or through SSH, setup user permissions, basic security and network management.

###Network
This guide will be making use of a static IP address to manage the network, if you have a dynamic IP address “an address that changes frequently”, you may have to make use of DNS services.
It is untested here, but a service such as No-IP may be able to assist as a static IP vendor for your Dynamic IP address.

###SFTP
Secure File Transfer Protocol Clients (SFTP) such as WinSCP will be required to perform SFTP actions and to generate PuTTY SSH Keys that will be used for accessing the web server.

##Packages, installation & setup


