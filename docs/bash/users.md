# Users

> Courtesy of [cyberciti.biz](https://www.cyberciti.biz/faq/create-a-user-account-on-ubuntu-linux/)

*Introduction*: By default, the cloud server comes with a user named ```ubuntu```. You can use such primary user account for sysadmin tasks on Ubuntu. However, sometimes you need to add a user account on Ubuntu for additional sysadmin tasks. This page shows how to create a regular user account or sysadmin account on the Ubuntu server.

## $ adduser 

**Steps to create a user account on Ubuntu Linux**
1. Open the terminal application  
2. Log in to remote box by running the ssh user@your-ubuntu-box-ip  
3. To add a new user in Ubuntu run ```sudo adduser userNameHere ```
4. Enter password and other needed info to create a user account on Ubuntu server  
5. New username would be added to /etc/passwd file, and encrypted password stored in the /etc/shadow file  

### Example
```bash
bbearce@bbearce-XPS-15-9560:~$ sudo adduser vivek
[sudo] password for bbearce: 
Adding user `vivek' ...
Adding new group `vivek' (1001) ...
Adding new user `vivek' (1001) with group `vivek' ...
Creating home directory `/home/vivek' ...
Copying files from `/etc/skel' ...
Enter new UNIX password: 
Retype new UNIX password: 
passwd: password updated successfully
Changing the user information for vivek
Enter the new value, or press ENTER for the default
	Full Name []: 
	Room Number []: 
	Work Phone []: 
	Home Phone []: 
	Other []: 
Is the information correct? [Y/n] y

```

### Verification
Use the grep command or cat command as follows:
```bash
$ cat /etc/passwd
.
.
.
vivek:x:1001:1001:,,,:/home/vivek:/bin/bash
```
or
```bash
$ grep '^vivek' /etc/passwd
vivek:x:1001:1001:,,,:/home/vivek:/bin/bash
```

## $ userdel

### Remove the user
1. Log in to your server via SSH.  
2. Switch to the root user:  
```$ sudo su -```  
3. Use the userdel command to remove the old user:  
```$ userdel <user's username>```  
4. Optional: You can also delete that user's home directory and mail spool by using the -r flag with the command:  
```$ userdel -r user's username```  

> Warning: Only delete a user's home directory if you are certain you no longer need their files or mail.

