# Linux Configure sudo

We have some users on all app servers in `Stratos Datacenter`. Some of them have been assigned some new roles and responsibilities, therefore their users need to be upgraded with sudo access so that they can perform admin level tasks

a. Provide sudo access to user `kirsty` on all app servers.

b. Make sure you have set up password-less sudo for the user.



My Solution:

```
# Login and run on all servers the following
sudo visudo

---
kirsty  ALL=(ALL)       NOPASSWD: ALL
---
thor@jumphost ~$ ssh tony@172.16.238.10
The authenticity of host '172.16.238.10 (172.16.238.10)' can't be established.
ED25519 key fingerprint is SHA256:AAxg+mxybenE1pbZiQXCX1EFVBJ9vJhHy3fz+j1wlF4.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '172.16.238.10' (ED25519) to the list of known hosts.
tony@172.16.238.10's password: 
[tony@stapp01 ~]$ id kirsty
uid=1002(kirsty) gid=1002(kirsty) groups=1002(kirsty)
[tony@stapp01 ~]$ sudo visudo

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for tony: 
[tony@stapp01 ~]$ exit
logout
Connection to 172.16.238.10 closed.
thor@jumphost ~$ ssh steve@172.16.238.11
The authenticity of host '172.16.238.11 (172.16.238.11)' can't be established.
ED25519 key fingerprint is SHA256:GbNa+7GBGdTaPQLVGW3wriPJV7QOTC0FkB95HIaIGdM.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '172.16.238.11' (ED25519) to the list of known hosts.
steve@172.16.238.11's password: 
[steve@stapp02 ~]$ sudo visudo 

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for steve: 
[steve@stapp02 ~]$ exit
logout
Connection to 172.16.238.11 closed.
thor@jumphost ~$ ssh banner@172.16.238.12
The authenticity of host '172.16.238.12 (172.16.238.12)' can't be established.
ED25519 key fingerprint is SHA256:KU5GSJwsU+mUFrPyoPVvqfLUTCq1JxZtxR0e16ZzOcE.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes 
Warning: Permanently added '172.16.238.12' (ED25519) to the list of known hosts.
banner@172.16.238.12's password: 
Permission denied, please try again.
banner@172.16.238.12's password: 
Last failed login: Mon Jul 21 17:45:27 UTC 2025 from 172.16.238.3 on ssh:notty
There was 1 failed login attempt since the last successful login.
[banner@stapp03 ~]$ sudo visudo 

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for banner: 
[banner@stapp03 ~]$ sudo systemctl restart sshd
[banner@stapp03 ~]$ exit
logout
Connection to 172.16.238.12 closed.
thor@jumphost ~$ ssh steve@172.16.238.11
steve@172.16.238.11's password: 
Last login: Mon Jul 21 17:43:59 2025 from 172.16.238.3
[steve@stapp02 ~]$ sudo systemctl restart sshd
[sudo] password for steve: 
[steve@stapp02 ~]$ exit
logout
Connection to 172.16.238.11 closed.
thor@jumphost ~$ ssh tony@172.16.238.10
tony@172.16.238.10's password: 
Last login: Mon Jul 21 17:42:53 2025 from 172.16.238.3
[tony@stapp01 ~]$ sudo systemctl restart sshd
[sudo] password for tony: 
[tony@stapp01 ~]$ exit
logout
Connection to 172.16.238.10 closed.
```
