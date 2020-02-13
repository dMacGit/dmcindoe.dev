---
title: "Remote Server Key login and Lockdown"
date: 2019-12-29T15:59:01+13:00
omit_header_text: true
draft: false
tags: ["Home-Lab","Sysadmin","Linux","Ubuntu","SSH","Keys"]
categories: ["Linux-Guide", "Home-Lab", "How-to"]
---
# Quick guide on Pushing SSH keys to servers Securing SSH.

In order to complete this, you will need to have made sure you have a user account setup with admin/root privileges. See my other Guide "[Admin account creation in Linux]()".

For this guide I will be using the account called **username**.

### Generate your SSH Key

On your Local system generate your SSH key, run the following:

``` bash
ssh-keygen -t rsa # Change '-t rsa' to your prefered Cryptographic Algorithm, or leave blank
```

``` bash
username@local:~$ ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/home/username/.ssh/id_rsa): /home/username/.ssh/id_rsa
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/username/.ssh/id_rsa.
Your public key has been saved in /home/username/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:hF/ZH6m4xn4S0djnUGele03tEusfm3lgn6FRlmY9sp8 username@local
The key's randomart image is:
+---[RSA 2048]----+
|                o|
|       .   o  .o+|
|      . . o+..=o=|
|       o .o.++.#o|
|        S ...+% =|
|         ... +++ |
|          +. .=++|
|         o. .. EB|
|          .o   +o|
+----[SHA256]-----+

```

### Push Key to Remote Server

Now that your key is generated, push it to the remote server:

``` bash
username@local:~$ ssh-copy-id -i ~/home/username/.ssh/id_rsa username@remote.machine
```
This may prompt you to enter the password for your user, once done the key should be saved.

Now that it is copied, it is good practise to test it.

``` bash
username@local:~$ ssh -i ~/.ssh/id_rsa username@remote.machine
```
You should now be logged in, without needing to type the password.

### Disable SSH Password Login on Remote Machine

Login to you're remote machine. Switch to root, if not already doing so.

Locate the sshd_config file, in Debian, this should be under 
``` bash
/etc/ssh/sshd_config 
```

Open this in the text **editor of you choice** (*Im using nanao in this example*)

``` bash
username@remote:~$ sudo nano /etc/ssh/sshd_config
```

Find and edit the following line peramters:

- PermitRootLogin no

- ChallengeResponseAuthentication no

- PasswordAuthentication no

- UsePAM no

Once saved, you'll need to reload the **ssh service**

``` bash
username@remote:~$ service ssh restart
```
OR

``` bash
username@reomte:~$ systemctl restart ssh
```

### SSH Verification

Now that is done, try and test it by logging in; first as root, then by your new user (without key). This should fail both times.

For root login test from local machine:

``` bash
username@local:~$ ssh root@remote
Permission denied (publickey).
```

And now by trying to force login by password

``` bash
username@local:~$ ssh username@remote -o PubKeyAuthentication=no
Permission denied (publickey).
```

Testing still works with SSH key:

``` bash
username@local:~$ ssh username@remote
Last login: Sat Aug 24 03:20:32 2019 from local.example.org
[username@remote ~]$
```