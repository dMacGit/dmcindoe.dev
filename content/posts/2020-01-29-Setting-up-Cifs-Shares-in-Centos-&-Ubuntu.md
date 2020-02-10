---
title: "Setting Up CIFS Shares in Centos & ubuntu"
date: 2020-01-29T15:59:01+13:00
omit_header_text: true
draft: false
tags: ["Home-Lab","Sysadmin","Linux","Ubuntu","Centos","CIFS"]
categories: ["Home-Lab","Linux-Guide", "How-to"]
---

# Guide to Cifs setup and Mounted on Boot.

This breif guide will go over installing the required utils to get samba installed and running,
as well as mounting a cifs share on boot in the fstab file.
I will be using Centos as the host environment, for Ubuntu, just replace "yum install" with "apt-get install".

## Installing samba & support libraries:

You will need to grab and install the Samba client as well as the cifs-utils package.

``` shell
yum install samba-client samba-common cifs-utils -y
```

## Configuring fstab.

Now that the packages are installed, we can write out our config in the fstab file.
At this point we need to decide where we want the mounted share to reside within our linux filesystem.
Generally remote & local shares can be mounted in the /mnt or /media direcory.
I mount remote shares in /mnt, so we will be using that dir in the guide.

Navagate to /mnt and create a directory/folder that you want the share to be mounted in.

``` shell
sudo mkdir /winshare
```

Now we can edit the fstab file.

Open /etc/fstab in you prefered editor (Im using nano).

``` shell 
sudo nano /etc/fstab
```

Now look over the below as a guide, before using the next step as an example:

```
//{Remote-host}/{share-directory} /mnt/{winshare} cifs uid={id},user={user-name},password={pass},iocharset=utf8 0 0
```

An exmaple for a guest user, with read/write access to a /Movies share would be:

```
//192.168.2.200/Movies /mnt/winshare/Movies cifs uid=1000,user=guest,password=guest,iocharset=utf8 0 0
```