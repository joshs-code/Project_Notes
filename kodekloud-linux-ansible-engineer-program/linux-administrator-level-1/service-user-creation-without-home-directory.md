# Service User Creation without Home Directory

In response to the latest tool implementation at `xFusionCorp Industries`, the system admins require the creation of a service user account. Here are the specifics:

Create a user named `ammar` in `App Server 1` without a home directory



My Solution:

```
thor@jumphost ~$ ssh tony@172.16.238.10
The authenticity of host '172.16.238.10 (172.16.238.10)' can't be established.
ED25519 key fingerprint is SHA256:E3fBFg1C7qELjHIO6rG6yaMH8HH3Qa+KMJDwL09KGig.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '172.16.238.10' (ED25519) to the list of known hosts.
tony@172.16.238.10's password: 
[tony@stapp01 ~]$ adduser --no-create-home ammar
adduser: Permission denied.
adduser: cannot lock /etc/passwd; try again later.
[tony@stapp01 ~]$ sudo adduser --no-create-home ammar

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for tony: 
[tony@stapp01 ~]$ tail -n 1 /etc/passwd
ammar:x:1002:1002::/home/ammar:/bin/bash
[tony@stapp01 ~]$ 
```
