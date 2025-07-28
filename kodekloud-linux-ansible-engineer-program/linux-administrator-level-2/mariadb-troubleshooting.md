# MariaDB Troubleshooting

There is a critical issue going on with the `Nautilus` application in `Stratos DC`. The production support team identified that the application is unable to connect to the database. After digging into the issue, the team found that mariadb service is down on the database server.

Look into the issue and fix the same.



My Solution:

```
thor@jumphost ~$ ssh peter@172.16.239.10
The authenticity of host '172.16.239.10 (172.16.239.10)' can't be established.
ED25519 key fingerprint is SHA256:pzDAOkG9wLy02nPl56K7K1f5U4bB99rweyYNBWXFNKs.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '172.16.239.10' (ED25519) to the list of known hosts.
peter@172.16.239.10's password: 
[peter@stdb01 ~]$ sudo systemctl status mariadb

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for peter: 
○ mariadb.service - MariaDB 10.5 database server
     Loaded: loaded (/usr/lib/systemd/system/mariadb.service; enabled; preset: disabled)
     Active: inactive (dead) since Thu 2025-07-24 13:11:46 UTC; 1min 17s ago
   Duration: 6.467s
       Docs: man:mariadbd(8)
             https://mariadb.com/kb/en/library/systemd/
    Process: 890 ExecStartPre=/usr/libexec/mariadb-check-socket (code=exited, status=0/SUCCESS)
    Process: 1099 ExecStartPre=/usr/libexec/mariadb-prepare-db-dir mariadb.service (code=exited, status=0/SUCCESS)
    Process: 1463 ExecStart=/usr/libexec/mariadbd --basedir=/usr $MYSQLD_OPTS $_WSREP_NEW_CLUSTER (code=exited, status=0/SUCCESS)
    Process: 1505 ExecStartPost=/usr/libexec/mariadb-check-upgrade (code=exited, status=0/SUCCESS)
   Main PID: 1463 (code=exited, status=0/SUCCESS)
     Status: "MariaDB server is down"

Jul 24 13:11:46 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Got notification message from PID 1463 (STATUS=Free innodb buffer pool, EXTEND_TIMEOUT_USEC=30000000)
Jul 24 13:11:46 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Got notification message from PID 1463 (STATUS=MariaDB server is down)
Jul 24 13:11:46 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Got notification message from PID 1463 (STATUS=MariaDB server is down)
Jul 24 13:11:46 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Child 1463 belongs to mariadb.service.
Jul 24 13:11:46 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Main process exited, code=exited, status=0/SUCCESS (success)
Jul 24 13:11:46 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Deactivated successfully.
Jul 24 13:11:46 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Service restart not allowed.
Jul 24 13:11:46 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Changed stop-sigterm -> dead
Jul 24 13:11:46 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Job 114 mariadb.service/stop finished, result=done
Jul 24 13:11:46 stdb01.stratos.xfusioncorp.com systemd[1]: Stopped MariaDB 10.5 database server.
[peter@stdb01 ~]$ sudo systemctl enable  mariadb
....
[peter@stdb01 ~]$ ls /var/log/mariadb/
ls: cannot open directory '/var/log/mariadb/': Permission denied
[peter@stdb01 ~]$ sudo ls /var/log/mariadb/
mariadb.log
[peter@stdb01 ~]$ sudo less /var/log/mariadb/mariadb.log
sudo: less: command not found
[peter@stdb01 ~]$ sudo cat /var/log/mariadb/mariadb.log
.....
[peter@stdb01 ~]$ ls -la /run/mariadb/mariadb.pid
ls: cannot access '/run/mariadb/mariadb.pid': No such file or directory
[peter@stdb01 ~]$ ls -a /run/mariadb/mariadb.pid
ls: cannot access '/run/mariadb/mariadb.pid': No such file or directory
[peter@stdb01 ~]$ ls -a /run/mariadb/
.  ..
[peter@stdb01 ~]$ ls -ld /run/mariadb/
drwxr-xr-x 2 root mysql 40 Jul 24 13:11 /run/mariadb/
[peter@stdb01 ~]$ sudo chmod g+rwx /run/mariadb/
[peter@stdb01 ~]$ ls -ld /run/mariadb/
drwxrwxr-x 2 root mysql 40 Jul 24 13:11 /run/mariadb/
[peter@stdb01 ~]$ sudo systemctl restart  mariadb
[peter@stdb01 ~]$ sudo systemctl status mariadb
● mariadb.service - MariaDB 10.5 database server
     Loaded: loaded (/usr/lib/systemd/system/mariadb.service; enabled; preset: disabled)
     Active: active (running) since Thu 2025-07-24 13:17:47 UTC; 6s ago
       Docs: man:mariadbd(8)
             https://mariadb.com/kb/en/library/systemd/
    Process: 2782 ExecStartPre=/usr/libexec/mariadb-check-socket (code=exited, status=0/SUCCESS)
    Process: 2826 ExecStartPre=/usr/libexec/mariadb-prepare-db-dir mariadb.service (code=exited, status=0/SUCCESS)
    Process: 2957 ExecStartPost=/usr/libexec/mariadb-check-upgrade (code=exited, status=0/SUCCESS)
   Main PID: 2921 (mariadbd)
     Status: "Taking your SQL requests now..."
      Tasks: 16 (limit: 411434)
     Memory: 96.9M
     CGroup: /docker/f77b6d5bd6c2b7a41a47d55dd28048af36287f661f148fbe54d8ae6677e0ab48/system.slice/mariadb.service
             └─2921 /usr/libexec/mariadbd --basedir=/usr

Jul 24 13:17:47 stdb01.stratos.xfusioncorp.com systemd[2957]: Remounted /run/systemd/unit-root/run/systemd/incoming.
Jul 24 13:17:47 stdb01.stratos.xfusioncorp.com systemd[2957]: Remounted /run/systemd/unit-root/run/credentials.
Jul 24 13:17:47 stdb01.stratos.xfusioncorp.com systemd[2957]: mariadb.service: Executing: /usr/libexec/mariadb-check-upgrade
Jul 24 13:17:47 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Child 2957 belongs to mariadb.service.
Jul 24 13:17:47 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Control process exited, code=exited, status=0/SUCCESS (success)
Jul 24 13:17:47 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Got final SIGCHLD for state start-post.
Jul 24 13:17:47 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Changed start-post -> running
Jul 24 13:17:47 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Job 283 mariadb.service/start finished, result=done
Jul 24 13:17:47 stdb01.stratos.xfusioncorp.com systemd[1]: Started MariaDB 10.5 database server.
Jul 24 13:17:47 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Failed to send unit change signal for mariadb.service: Connection reset by peer
[peter@stdb01 ~]$ sudo systemctl enable --now mariadb
```
