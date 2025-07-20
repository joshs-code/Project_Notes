# Linux SSH Authentication

The system admins team of xFusionCorp Industries has set up some scripts on jump host that run on regular intervals and perform operations on all app servers in Stratos Datacenter. To make these scripts work properly we need to make sure the thor user on jump host has password-less SSH access to all app servers through their respective sudo users (i.e tony for app server 1). Based on the requirements, perform the following:

Set up a password-less authentication from user thor on jump host to all app servers through their respective sudo users.



My Solution:

```
thor@jumphost ~$ ssh-keygen -t rsa -b 4096
Generating public/private rsa key pair.
Enter file in which to save the key (/home/thor/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/thor/.ssh/id_rsa
Your public key has been saved in /home/thor/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:Ajx613SitvV1cK/f6a+hLsEEBlUQQihR57Z1c9V076w thor@jumphost.stratos.xfusioncorp.com
The key's randomart image is:
+---[RSA 4096]----+
|  .o.o=o++.   .oo|
|  ...o .o    .  +|
|   .+ o.+.+ o . .|
|   . + * +.o o + |
|  . . * So  . . +|
|   . o + .o. . o |
|      .   ..  E  |
|          .  . oo|
|           oo o+=|
+----[SHA256]-----+
thor@jumphost ~$ ssh-copy-id tony@172.16.238.10
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/thor/.ssh/id_rsa.pub"
The authenticity of host '172.16.238.10 (172.16.238.10)' can't be established.
ED25519 key fingerprint is SHA256:bCfJAoW9kZPAlBcmQ7feIKE88Ken6LjXLDWUflkr6kE.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
tony@172.16.238.10's password: 
Permission denied, please try again.
tony@172.16.238.10's password: 
Permission denied, please try again.
tony@172.16.238.10's password: 

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'tony@172.16.238.10'"
and check to make sure that only the key(s) you wanted were added.

thor@jumphost ~$ ssh-copy-id steve@172.16.238.11
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/thor/.ssh/id_rsa.pub"
The authenticity of host '172.16.238.11 (172.16.238.11)' can't be established.
ED25519 key fingerprint is SHA256:kUdcnFiVr9yaSWZFJk+kNrvuH1Wvrun304o9quap11w.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
steve@172.16.238.11's password: 

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'steve@172.16.238.11'"
and check to make sure that only the key(s) you wanted were added.

thor@jumphost ~$ ssh-copy-id banner@172.16.238.12
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/thor/.ssh/id_rsa.pub"
The authenticity of host '172.16.238.12 (172.16.238.12)' can't be established.
ED25519 key fingerprint is SHA256:sXUo0xFhJiyqIgeLMug9RVUv4fwOtA13+Rak7OF423Y.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
banner@172.16.238.12's password: 
Permission denied, please try again.
banner@172.16.238.12's password: 
Permission denied, please try again.
banner@172.16.238.12's password: 

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'banner@172.16.238.12'"
and check to make sure that only the key(s) you wanted were added.

thor@jumphost ~$ ssh tony@172.16.238.10
Last failed login: Fri Jul 18 12:36:16 UTC 2025 from 172.16.238.3 on ssh:notty
There were 2 failed login attempts since the last successful login.
[tony@stapp01 ~]$ exit
logout
Connection to 172.16.238.10 closed.
thor@jumphost ~$ ssh steve@172.16.238.11
[steve@stapp02 ~]$ exit
logout
Connection to 172.16.238.11 closed.
thor@jumphost ~$ ssh banner@172.16.238.12
Last failed login: Fri Jul 18 12:37:08 UTC 2025 from 172.16.238.3 on ssh:notty
There were 2 failed login attempts since the last successful login.
[banner@stapp03 ~]$ exit
logout
Connection to 172.16.238.12 closed.
```
