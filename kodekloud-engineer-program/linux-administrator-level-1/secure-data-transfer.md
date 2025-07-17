# Secure Data Transfer

A `Nautilus` developer has stored confidential data on the jump host within `Stratos DC`. To ensure security and compliance, this data must be transferred to one of the app servers. Given developers lack direct access to these servers, the system admin team has been enlisted for assistance.

Copy `/tmp/nautilus.txt.gpg` file from jump server to `App Server 1` placing it in the directory `/home/nfsdata`.



My Solution:

```
thor@jumphost ~$ scp /tmp/nautilus.txt.gpg tony@172.16.238.10:/home/nfsdata
The authenticity of host '172.16.238.10 (172.16.238.10)' can't be established.
ED25519 key fingerprint is SHA256:tBPpgAZOpscagl9B6y5/D2Yv7rioZYKPnh6FC3/M5Tg.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '172.16.238.10' (ED25519) to the list of known hosts.
tony@172.16.238.10's password: 
nautilus.txt.gpg                                                          100%  105   665.8KB/s   00:00    
thor@jumphost ~$ ssh tony@172.16.238.10
tony@172.16.238.10's password: 
[tony@stapp01 ~]$ sudo ls /home/

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for tony: 
ansible  nfsdata  tony
[tony@stapp01 ~]$ 
```
