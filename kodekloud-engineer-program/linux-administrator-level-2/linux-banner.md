# Linux Banner

During the monthly compliance meeting, it was pointed out that several servers in the `Stratos DC` do not have a valid banner. The security team has provided serveral approved templates which should be applied to the servers to maintain compliance. These will be displayed to the user upon a successful login.

Update the `message of the day` on all application and db servers for `Nautilus`. Make use of the approved template located at `/home/thor/nautilus_banner` on jump host



My Solution:

```
thor@jumphost ~$ ssh tony@172.16.238.10
The authenticity of host '172.16.238.10 (172.16.238.10)' can't be established.
ED25519 key fingerprint is SHA256:qWc8r2XuRDq3BN4M0cggiLZQjwuYIYM+mfcpS8BJjas.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '172.16.238.10' (ED25519) to the list of known hosts.
tony@172.16.238.10's password: 
[tony@stapp01 ~]$ sudo vi /etc/motd

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
ED25519 key fingerprint is SHA256:j8G/YV/4hR7+w2S0tXoTyItB3yBjnYnZGDjBLK4IuKk.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '172.16.238.11' (ED25519) to the list of known hosts.
steve@172.16.238.11's password: 
[steve@stapp02 ~]$ sudo vi /etc/motd

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
ED25519 key fingerprint is SHA256:gzjCFDDYUKBgh/dqlehsuMlv8HGLEOzg/bWfbgow+fk.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '172.16.238.12' (ED25519) to the list of known hosts.
banner@172.16.238.12's password: 
Permission denied, please try again.
banner@172.16.238.12's password: 
Permission denied, please try again.
banner@172.16.238.12's password: 
Last failed login: Thu Jul 17 12:03:18 UTC 2025 from 172.16.238.2 on ssh:notty
There were 2 failed login attempts since the last successful login.
[banner@stapp03 ~]$ sudo vi /etc/motd

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for banner: 
[banner@stapp03 ~]$ exit
logout
Connection to 172.16.238.12 closed.
thor@jumphost ~$ ssh peter@172.16.239.10        
The authenticity of host '172.16.239.10 (172.16.239.10)' can't be established.
ED25519 key fingerprint is SHA256:BQ1KQK2+viWZwsc+axcojb1dj1AnsbhjI6Zt/72dkZ4.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '172.16.239.10' (ED25519) to the list of known hosts.
peter@172.16.239.10's password: 
[peter@stdb01 ~]$ sudo vi /etc/motd

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for peter: 
Sorry, try again.
[sudo] password for peter: 
[peter@stdb01 ~]$ exit
logout
Connection to 172.16.239.10 closed.
```
