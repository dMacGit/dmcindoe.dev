---
title: "Creating a Sudo user account in linux"
date: 2019-12-29T15:59:01+13:00
omit_header_text: true
draft: false
tags: ["Home-Lab","Sysadmin","Linux","Ubuntu","Users"]
categories: ["Home-Lab","Linux-Guide", "How-to"]
---
# Creating a Sudo user account in linux.

This is a quick guide in how to create a suer with "Sudo" priviliges in centos.

First step is to login to the machine and switch to the root user.

> su root

Now we can create a new user and assign sudo wrights. (Username, being the new user.

> adduser username

This will start the account creation process, and you should see similar output to below.
In this first output it will prompt you to enter a password for the user.

```shell script
Adding user `username' ...
Adding new group `username' (1001) ...
Adding new user `username' (1001) with group `username' ...
Creating home directory `/home/username' ...
Copying files from `/etc/skel' ...
New password:
Retype new password:
passwd: password updated successfully
```

Once done, it will ask for more details.

```shell script
Changing the user information for username
Enter the new value, or press ENTER for the default
    Full Name []:
    Room Number []:
    Work Phone []:
    Home Phone []:
    Other []:
Is the information correct? [Y/n]
```

Fill in what you need, or leave empty to use defaults.

This user is now created, however we also need to add them to the "sudo" group.

To do this simply execute the following command (Username, being the user):

> usermod -aG sudo username

This should now be all setup. But it is a good pracitse to test the account.

Firstly switch to the new account:

> su - username

Run the "whoami" command under "sudo":

> sudo whoami

If the user has "sudo" access, then the output should be "root"

```shell script
root
```

In order to use "sudo", you will need to prefix the command with it, like normal.

```shell script
sudo ls -l /root
```
And the first time you use it in a session, it will prompt you for the "sudo" password.

```shell script
[sudo] password for username
```