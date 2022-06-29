## Useful Commands
### Nano
```
Open File: $ sudo nano /directory/filename.ext
Save File: CTRL + S
Search: CTRL + W
```
### User Groups
```
Get all user groups: $ sudo getent group
Delete Groups: $ sudo groupdel groupName
User groups: $ sudo groups username
Add User to group: $ sudo usermod -a -G groupname username
Remove User from group: $ sudo gpasswd –delete username group
```
### Directory Related
```
Make Directory: $ sudo mkdir /path/of/directory/folder1
Remove Directory: $ sudo rmdir /path/of/directory/folder1
Remove Directory and Contents: $ sudo rm -r /path/of/directory/folder1
Change Permissions: $ sudo chmod 775 /path/of/directory/folder1
Change Ownership: $ sudo chown username:username  /path/of/directory/folder1
Change Group ownership: $ sudo chgrp userGroupName -R /path/of/directory
```

### Debug: Active Process
```
Locate active process
Search for Port 8080 processes: $ sudo ps -ef | grep ':8080'

Kill Certbot Process
Error: Another instance of Certbot is already running.
Find active Certbot processes: $ ps -ef | grep certbot

Kill Certbot processes
Note: The process id shown in this image may be different from your own.
$ sudo kill 47666
```
![image](https://user-images.githubusercontent.com/50721672/176333367-e3a35347-c348-4123-aaa6-fe2634bf3c26.png)
