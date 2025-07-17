# Group Creation and User Assignment

The system admin team at `xFusionCorp Industries` has streamlined access management by implementing group-based access control. Here's what you need to do:

a. Create a group named `nautilus_developers` across all App servers within the `Stratos Datacenter`.

b. Add the user `kano` into the `nautilus_developers` group on all App servers. If the user doesn't exist, create it as well.



My Solution:

```

thor@jumphost ~$ ssh tony@172.16.238.10
The authenticity of host '172.16.238.10 (172.16.238.10)' can't be established.
ED25519 key fingerprint is SHA256:4+2A/FoAVDBr8t4FzjdQi6eU21MJF5WjJijoagVX7ew.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '172.16.238.10' (ED25519) to the list of known hosts.
tony@172.16.238.10's password: 
[tony@stapp01 ~]$ sudo groupadd nautilus_developers

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for tony: 
[tony@stapp01 ~]$ grep kano /etc/passwd
[tony@stapp01 ~]$ adduser kano 
adduser: Permission denied.
adduser: cannot lock /etc/passwd; try again later.
[tony@stapp01 ~]$ sudo adduser kano
[tony@stapp01 ~]$ sudo usermod -aG nautilus_developers kano
[tony@stapp01 ~]$ exit
logout
Connection to 172.16.238.10 closed.
thor@jumphost ~$ ssh steve@172.16.238.11
The authenticity of host '172.16.238.11 (172.16.238.11)' can't be established.
ED25519 key fingerprint is SHA256:iuMlAeYlJ3OSiVNR49IOfruOGGayHJ+TrnIIjm5HUTw.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '172.16.238.11' (ED25519) to the list of known hosts.
steve@172.16.238.11's password: 
[steve@stapp02 ~]$ sudo groupadd nautilus_developers

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for steve: 
[steve@stapp02 ~]$ sudo adduser kano
[steve@stapp02 ~]$ sudo usermod -aG nautilus_developers kano
[steve@stapp02 ~]$ exiy
-bash: exiy: command not found
[steve@stapp02 ~]$ exit
logout
Connection to 172.16.238.11 closed.
thor@jumphost ~$ ssh banner@172.16.238.12
The authenticity of host '172.16.238.12 (172.16.238.12)' can't be established.
ED25519 key fingerprint is SHA256:t5GJeEVhhsuf0TJkM5biJ+udLEeL/tJ/wjneD+GWGgg.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '172.16.238.12' (ED25519) to the list of known hosts.
banner@172.16.238.12's password: 
[banner@stapp03 ~]$ sudo groupadd nautilus_developers

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for banner: 
[banner@stapp03 ~]$ sudo adduser kano
[banner@stapp03 ~]$ sudo usermod -aG nautilus_developers kano
[banner@stapp03 ~]$ exit
logout
Connection to 172.16.238.12 closed.
```
