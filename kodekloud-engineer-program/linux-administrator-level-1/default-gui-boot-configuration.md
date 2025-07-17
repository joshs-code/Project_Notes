# Default GUI Boot Configuration

With the installation of new tools on the app servers within the `Stratos Datacenter`, certain functionalities now necessitate graphical user interface (GUI) access.

Adjust the `default runlevel` on all App servers in `Stratos Datacenter` to enable GUI booting by default. It's imperative not to initiate a server reboot after completing this task.



My Solution:

```
thor@jumphost ~$ ssh tony@172.16.238.10 
The authenticity of host '172.16.238.10 (172.16.238.10)' can't be established.
ED25519 key fingerprint is SHA256:RRvzBwn4+xcqeaRWiB54YW0mclWjA80cWaA70AHM7xA.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '172.16.238.10' (ED25519) to the list of known hosts.
tony@172.16.238.10's password: 
Permission denied, please try again.
tony@172.16.238.10's password: 
Permission denied, please try again.
tony@172.16.238.10's password: 
Last failed login: Tue Jul 15 11:44:34 UTC 2025 from 172.16.238.3 on ssh:notty
There were 2 failed login attempts since the last successful login.
[tony@stapp01 ~]$ runlevel
N 5
[tony@stapp01 ~]$ sudo systemctl set-default graphical.target

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for tony: 
Removed "/etc/systemd/system/default.target".
Created symlink /etc/systemd/system/default.target → /usr/lib/systemd/system/graphical.target.
[tony@stapp01 ~]$ exit
logout
Connection to 172.16.238.10 closed.
thor@jumphost ~$ ssh steve@172.16.238.11
The authenticity of host '172.16.238.11 (172.16.238.11)' can't be established.
ED25519 key fingerprint is SHA256:rR9WgV5RXiu/0W6vseWCF4TBnBUqmtLcCv5+lPOl6Nw.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '172.16.238.11' (ED25519) to the list of known hosts.
steve@172.16.238.11's password: 
[steve@stapp02 ~]$ sudo systemctl set-default graphical.target

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for steve: 
Removed "/etc/systemd/system/default.target".
Created symlink /etc/systemd/system/default.target → /usr/lib/systemd/system/graphical.target.
[steve@stapp02 ~]$ exit
logout
Connection to 172.16.238.11 closed.
thor@jumphost ~$ ssh banner@172.16.238.12       
The authenticity of host '172.16.238.12 (172.16.238.12)' can't be established.
ED25519 key fingerprint is SHA256:+BcgLobXD3Vudxq1mUPTBMp2SUv1BtMsQ4F6V2ETTA0.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '172.16.238.12' (ED25519) to the list of known hosts.
banner@172.16.238.12's password: 
Permission denied, please try again.
banner@172.16.238.12's password: 
Permission denied, please try again.
banner@172.16.238.12's password: 
Last failed login: Tue Jul 15 11:47:56 UTC 2025 from 172.16.238.3 on ssh:notty
There were 2 failed login attempts since the last successful login.
[banner@stapp03 ~]$ sudo systemctl set-default graphical.target

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for banner: 
Removed "/etc/systemd/system/default.target".
Created symlink /etc/systemd/system/default.target → /usr/lib/systemd/system/graphical.target.
[banner@stapp03 ~]$ 
```
