# Linux Collaborative Directories

The Nautilus team doesn't want its data to be accessed by any of the other groups/teams due to security reasons and want their data to be strictly accessed by the `dbadmin` group of the team.

Setup a collaborative directory `/dbadmin/data` on `app server 2` in `Stratos Datacenter`.

The directory should be group owned by the group `dbadmin` and the group should own the files inside the directory. The directory should be `read/write/execute` to the user and group owners, and `others` should not have any access.



My Solution:

```
thor@jumphost ~$ ssh steve@172.16.238.11
The authenticity of host '172.16.238.11 (172.16.238.11)' can't be established.
ED25519 key fingerprint is SHA256:VBZWanV4nVGaaDphkfXrIQHpv7OjOpaJYrcXk8O+tD4.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '172.16.238.11' (ED25519) to the list of known hosts.
steve@172.16.238.11's password: 
Permission denied, please try again.
steve@172.16.238.11's password: 
Last failed login: Fri Jul 18 00:35:01 UTC 2025 from 172.16.238.3 on ssh:notty
There was 1 failed login attempt since the last successful login.
[steve@stapp02 ~]$ mkdir -p /dbadmin/data
mkdir: cannot create directory ‘/dbadmin’: Permission denied
[steve@stapp02 ~]$ sudo mkdir -p /dbadmin/data

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for steve: 
[steve@stapp02 ~]$ sudo chgrp dbadmin /dbadmin/data/
[steve@stapp02 ~]$ sudo chmod 770 /dbadmin/data/
[steve@stapp02 ~]$ ls -ld /dbadmin/data/
drwxrwx--- 2 root dbadmin 4096 Jul 18 00:35 /dbadmin/data/
[steve@stapp02 ~]$ 
```
