# Data Backup for Developer

Within the Stratos DC, the Nautilus storage server hosts a directory named `/data`, serving as a repository for various developers non-confidential data. Developer `rose` has requested a copy of their data stored in `/data/rose`. The System Admin team has provided the following steps to fulfill this request:

a. Create a compressed archive named `rose.tar.gz` of the `/data/rose` directory.

b. Transfer the archive to the `/home` directory on the Storage Server.



My Solution:

```
thor@jumphost ~$ ssh peter@172.16.239.10
ssh: connect to host 172.16.239.10 port 22: No route to host
thor@jumphost ~$ ssh natasha@172.16.238.15
The authenticity of host '172.16.238.15 (172.16.238.15)' can't be established.
ED25519 key fingerprint is SHA256:ECZLacdKPCfgqIbPnNarWrlaDhqRUWUqe1aFLBGdipI.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '172.16.238.15' (ED25519) to the list of known hosts.
natasha@172.16.238.15's password: 
[natasha@ststor01 ~]$ tar -czf rose.tar.gz /data/rose
tar: Removing leading `/' from member names
[natasha@ststor01 ~]$ ls
rose.tar.gz
[natasha@ststor01 ~]$ mv rose.tar.gz /home/
mv: cannot move 'rose.tar.gz' to '/home/rose.tar.gz': Permission denied
[natasha@ststor01 ~]$ sudo mv rose.tar.gz /home/

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for natasha: 
[natasha@ststor01 ~]$ sudo ls /home/
ansible  natasha  rose.tar.gz
```
