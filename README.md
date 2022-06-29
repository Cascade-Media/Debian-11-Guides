# Debian 11 Web Server Guide
Covering Packages, setup and management

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
About
Allows the system to update security packages (and other) updates automatically

Installation
```
$ sudo apt-get install unattended-upgrades apt-listchanges
```

OpenSSH Server
OpenSSH is required in order for the user to be able to remotely access the server through secure shell.
See: LinuxHint: Enable SSH Server on Debian

Installation & Setup
Install open ssh server
```
$ sudo apt-get install openssh-server
```

Enable the ssh server 
```
$ sudo systemctl enable ssh
```

Start SSH
```
$ sudo systemctl start ssh
```

Check SSH status
```
$ sudo systemctl status ssh
```

Security: disable Root SSH access
See: [MediaTemple: How do I disable SSH login for the root user?](https://mediatemple.net/community/products/dv/204643810/how-do-i-disable-ssh-login-for-the-root-user#:~:text=Verify%20that%20you%20can%20su,root%20with%20the%20admin%20user.&text=To%20disable%20root%20SSH%20login,with%20your%20favorite%20text%20editor.&text=Ensure%20that%20you%20are%20logged,yourself%20out%20of%20the%20server.)
The root user is a primary target when it comes to SSH access related attacks as it has the master authority over the system, It is advised that you disable Root access over SSH for this very reason in the event that an SSH private key is stolen and the account becomes compromised.

To disable SSH for root you will need to open the ssdh_config file.
Location: /etc/ssh/ssh_config

You are free to modify the file with any method available.
Once open you should modify the following.

From
```
#PermitRootLogin prohibit-password
```

To
```
PermitRootLogin no
```

Once done, save the file and close.
Root will now no longer be able to make use of SSH
Once Done we should create or modify an existing user
See: Secure Shell (SSH) & Secure File Transfer Protocol (SFTP) setup


