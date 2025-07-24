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
[peter@stdb01 ~]$ sudo systemctl restart  mariadb
Job for mariadb.service failed because the control process exited with error code.
See "systemctl status mariadb.service" and "journalctl -xeu mariadb.service" for details.
[peter@stdb01 ~]$ journalctl -xeu mariadb.service
Jul 24 13:11:36 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: start
ing held back, waiting for: basic.target
Jul 24 13:11:37 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Will 
spawn child (service_enter_start_pre): /usr/libexec/mariadb-check-socket
Jul 24 13:11:37 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Faile
d to reset devices.allow/devices.deny: Operation not permitted
Jul 24 13:11:37 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Faile
d to set 'trusted.invocation_id' xattr on control group /docker/f77b6d5bd6c2b7a41a47d55dd28048af36287f661f148fbe5
4d8ae6677e0ab48/system.slice/mariadb.service, ignoring: Operation not permitted
Jul 24 13:11:37 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Faile
d to remove 'trusted.delegate' xattr flag on control group /docker/f77b6d5bd6c2b7a41a47d55dd28048af36287f661f148f
be54d8ae6677e0ab48/system.slice/mariadb.service, ignoring: Operation not permitted
Jul 24 13:11:37 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: About
 to execute /usr/libexec/mariadb-check-socket
Jul 24 13:11:37 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Forke
d /usr/libexec/mariadb-check-socket as 890
Jul 24 13:11:37 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Chang
ed dead -> start-pre
Jul 24 13:11:37 stdb01.stratos.xfusioncorp.com systemd[1]: Starting MariaDB 10.5 database server...
░░ Subject: A start job for unit mariadb.service has begun execution
░░ Defined-By: systemd
░░ Support: https://access.redhat.com/support
░░ 
░░ A start job for unit mariadb.service has begun execution.
░░ 
░░ The job identifier is 59.
Jul 24 13:11:37 stdb01.stratos.xfusioncorp.com systemd[890]: PR_SET_MM_ARG_START 
failed: Operation not permitted
Jul 24 13:11:37 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: User 
lookup succeeded: uid=27 gid=27
Jul 24 13:11:37 stdb01.stratos.xfusioncorp.com systemd[890]: Bind-mounting / on /
run/systemd/unit-root (MS_BIND|MS_REC "")...
Jul 24 13:11:37 stdb01.stratos.xfusioncorp.com systemd[890]: Applying namespace m
ount on /run/systemd/unit-root/run/credentials
Jul 24 13:11:37 stdb01.stratos.xfusioncorp.com systemd[890]: Bind-mounting /run/s
ystemd/inaccessible/dir on /run/systemd/unit-root/run/credentials (MS_BIND|MS_REC "")...
Jul 24 13:11:37 stdb01.stratos.xfusioncorp.com systemd[890]: Successfully mounted
 /run/systemd/inaccessible/dir to /run/systemd/unit-root/run/credentials
Jul 24 13:11:37 stdb01.stratos.xfusioncorp.com systemd[890]: Applying namespace m
ount on /run/systemd/unit-root/run/systemd/incoming
Jul 24 13:11:37 stdb01.stratos.xfusioncorp.com systemd[890]: Followed source syml
inks /run/systemd/propagate/mariadb.service → /run/systemd/propagate/mariadb.service.
Jul 24 13:11:37 stdb01.stratos.xfusioncorp.com systemd[890]: Bind-mounting /run/s
ystemd/propagate/mariadb.service on /run/systemd/unit-root/run/systemd/incoming (MS_BIND "")...
Jul 24 13:11:37 stdb01.stratos.xfusioncorp.com systemd[890]: Failed to mount /run
/systemd/propagate/mariadb.service (type n/a) on /run/systemd/unit-root/run/systemd/incoming (MS_BIND ""): No suc
h file or directory
Jul 24 13:11:37 stdb01.stratos.xfusioncorp.com systemd[890]: Bind-mounting /run/s
ystemd/propagate/mariadb.service on /run/systemd/unit-root/run/systemd/incoming (MS_BIND "")...
Jul 24 13:11:37 stdb01.stratos.xfusioncorp.com systemd[890]: Successfully mounted
 /run/systemd/propagate/mariadb.service to /run/systemd/unit-root/run/systemd/incoming
Jul 24 13:11:37 stdb01.stratos.xfusioncorp.com systemd[890]: Applying namespace m
ount on /run/systemd/unit-root/tmp
Jul 24 13:11:37 stdb01.stratos.xfusioncorp.com systemd[890]: Bind-mounting /tmp/s
ystemd-private-567bda43f04743f0b238c00f2a27c76d-mariadb.service-Ra7h8V/tmp on /run/systemd/unit-root/tmp (MS_BIND
|MS_REC "")...
Jul 24 13:11:37 stdb01.stratos.xfusioncorp.com systemd[890]: Successfully mounted
 /tmp/systemd-private-567bda43f04743f0b238c00f2a27c76d-mariadb.service-Ra7h8V/tmp to /run/systemd/unit-root/tmp

Jul 24 13:11:37 stdb01.stratos.xfusioncorp.com systemd[890]: Applying namespace m
ount on /run/systemd/unit-root/var/tmp
Jul 24 13:11:37 stdb01.stratos.xfusioncorp.com systemd[890]: Bind-mounting /var/t
mp/systemd-private-567bda43f04743f0b238c00f2a27c76d-mariadb.service-8hQDnj/tmp on /run/systemd/unit-root/var/tmp 
(MS_BIND|MS_REC "")...
Jul 24 13:11:37 stdb01.stratos.xfusioncorp.com systemd[890]: Successfully mounted
 /var/tmp/systemd-private-567bda43f04743f0b238c00f2a27c76d-mariadb.service-8hQDnj/tmp to /run/systemd/unit-root/v
ar/tmp
Jul 24 13:11:37 stdb01.stratos.xfusioncorp.com systemd[890]: Remounted /run/syste
md/unit-root/run/credentials.
Jul 24 13:11:37 stdb01.stratos.xfusioncorp.com systemd[890]: Remounted /run/syste
md/unit-root/run/systemd/incoming.
Jul 24 13:11:37 stdb01.stratos.xfusioncorp.com systemd[890]: Remounted /run/syste
md/unit-root/run/credentials.
Jul 24 13:11:37 stdb01.stratos.xfusioncorp.com systemd[890]: mariadb.service: Exe
cuting: /usr/libexec/mariadb-check-socket
Jul 24 13:11:37 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Child
 890 belongs to mariadb.service.
Jul 24 13:11:37 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Contr
ol process exited, code=exited, status=0/SUCCESS (success)
░░ Subject: Unit process exited
░░ Defined-By: systemd
░░ Support: https://access.redhat.com/support
░░ 
░░ An ExecStartPre= process belonging to unit mariadb.service has exited.
░░ 
░░ The process' exit code is 'exited' and its exit status is 0.
Jul 24 13:11:37 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Runni
ng next control command for state start-pre.
Jul 24 13:11:37 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Will 
spawn child (service_run_next_control): /usr/libexec/mariadb-prepare-db-dir
Jul 24 13:11:37 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: About
 to execute /usr/libexec/mariadb-prepare-db-dir mariadb.service
Jul 24 13:11:37 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Forke
d /usr/libexec/mariadb-prepare-db-dir as 1099
Jul 24 13:11:37 stdb01.stratos.xfusioncorp.com systemd[1099]: PR_SET_MM_ARG_START
 failed: Operation not permitted
Jul 24 13:11:37 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: User 
lookup succeeded: uid=27 gid=27
Jul 24 13:11:37 stdb01.stratos.xfusioncorp.com systemd[1099]: Bind-mounting / on 
/run/systemd/unit-root (MS_BIND|MS_REC "")...
Jul 24 13:11:37 stdb01.stratos.xfusioncorp.com systemd[1099]: Applying namespace 
mount on /run/systemd/unit-root/run/credentials
Jul 24 13:11:37 stdb01.stratos.xfusioncorp.com systemd[1099]: Bind-mounting /run/
systemd/inaccessible/dir on /run/systemd/unit-root/run/credentials (MS_BIND|MS_REC "")...
Jul 24 13:11:37 stdb01.stratos.xfusioncorp.com systemd[1099]: Successfully mounte
d /run/systemd/inaccessible/dir to /run/systemd/unit-root/run/credentials
Jul 24 13:11:37 stdb01.stratos.xfusioncorp.com systemd[1099]: Applying namespace 
mount on /run/systemd/unit-root/run/systemd/incoming
Jul 24 13:11:37 stdb01.stratos.xfusioncorp.com systemd[1099]: Followed source sym
links /run/systemd/propagate/mariadb.service → /run/systemd/propagate/mariadb.service.
Jul 24 13:11:37 stdb01.stratos.xfusioncorp.com systemd[1099]: Bind-mounting /run/
systemd/propagate/mariadb.service on /run/systemd/unit-root/run/systemd/incoming (MS_BIND "")...
Jul 24 13:11:37 stdb01.stratos.xfusioncorp.com systemd[1099]: Successfully mounte
d /run/systemd/propagate/mariadb.service to /run/systemd/unit-root/run/systemd/incoming
Jul 24 13:11:37 stdb01.stratos.xfusioncorp.com systemd[1099]: Applying namespace 
mount on /run/systemd/unit-root/tmp
Jul 24 13:11:37 stdb01.stratos.xfusioncorp.com systemd[1099]: Bind-mounting /tmp/
systemd-private-567bda43f04743f0b238c00f2a27c76d-mariadb.service-Ra7h8V/tmp on /run/systemd/unit-root/tmp (MS_BIN
D|MS_REC "")...
Jul 24 13:11:37 stdb01.stratos.xfusioncorp.com systemd[1099]: Successfully mounte
d /tmp/systemd-private-567bda43f04743f0b238c00f2a27c76d-mariadb.service-Ra7h8V/tmp to /run/systemd/unit-root/tmp

Jul 24 13:11:37 stdb01.stratos.xfusioncorp.com systemd[1099]: Applying namespace 
mount on /run/systemd/unit-root/var/tmp
Jul 24 13:11:37 stdb01.stratos.xfusioncorp.com systemd[1099]: Bind-mounting /var/
tmp/systemd-private-567bda43f04743f0b238c00f2a27c76d-mariadb.service-8hQDnj/tmp on /run/systemd/unit-root/var/tmp
 (MS_BIND|MS_REC "")...
Jul 24 13:11:37 stdb01.stratos.xfusioncorp.com systemd[1099]: Successfully mounte
d /var/tmp/systemd-private-567bda43f04743f0b238c00f2a27c76d-mariadb.service-8hQDnj/tmp to /run/systemd/unit-root/
var/tmp
Jul 24 13:11:37 stdb01.stratos.xfusioncorp.com systemd[1099]: Remounted /run/syst
emd/unit-root/run/credentials.
Jul 24 13:11:37 stdb01.stratos.xfusioncorp.com systemd[1099]: Remounted /run/syst
emd/unit-root/run/systemd/incoming.
Jul 24 13:11:37 stdb01.stratos.xfusioncorp.com systemd[1099]: Remounted /run/syst
emd/unit-root/run/credentials.
Jul 24 13:11:37 stdb01.stratos.xfusioncorp.com systemd[1099]: mariadb.service: Ex
ecuting: /usr/libexec/mariadb-prepare-db-dir mariadb.service
Jul 24 13:11:38 stdb01.stratos.xfusioncorp.com mariadb-prepare-db-dir[1099]: Initializing MariaDB database
Jul 24 13:11:39 stdb01.stratos.xfusioncorp.com mariadb-prepare-db-dir[1403]: Two all-privilege accounts were crea
ted.
Jul 24 13:11:39 stdb01.stratos.xfusioncorp.com mariadb-prepare-db-dir[1403]: One is root@localhost, it has no pas
sword, but you need to
Jul 24 13:11:39 stdb01.stratos.xfusioncorp.com mariadb-prepare-db-dir[1403]: be system 'root' user to connect. Us
e, for example, sudo mysql
Jul 24 13:11:39 stdb01.stratos.xfusioncorp.com mariadb-prepare-db-dir[1403]: The second is mysql@localhost, it ha
s no password either, but
Jul 24 13:11:39 stdb01.stratos.xfusioncorp.com mariadb-prepare-db-dir[1403]: you need to be the system 'mysql' us
er to connect.
Jul 24 13:11:39 stdb01.stratos.xfusioncorp.com mariadb-prepare-db-dir[1403]: After connecting you can set the pas
sword, if you would need to be
Jul 24 13:11:39 stdb01.stratos.xfusioncorp.com mariadb-prepare-db-dir[1403]: able to connect as any of these user
s with a password and without sudo
Jul 24 13:11:39 stdb01.stratos.xfusioncorp.com mariadb-prepare-db-dir[1403]: See the MariaDB Knowledgebase at htt
ps://mariadb.com/kb
Jul 24 13:11:39 stdb01.stratos.xfusioncorp.com mariadb-prepare-db-dir[1403]: Please report any problems at https:
//mariadb.org/jira
Jul 24 13:11:39 stdb01.stratos.xfusioncorp.com mariadb-prepare-db-dir[1403]: The latest information about MariaDB
 is available at https://mariadb.org/.
Jul 24 13:11:39 stdb01.stratos.xfusioncorp.com mariadb-prepare-db-dir[1403]: Consider joining MariaDB's strong an
d vibrant community:
Jul 24 13:11:39 stdb01.stratos.xfusioncorp.com mariadb-prepare-db-dir[1403]: https://mariadb.org/get-involved/
Jul 24 13:11:39 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Child
 1099 belongs to mariadb.service.
Jul 24 13:11:39 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Contr
ol process exited, code=exited, status=0/SUCCESS (success)
░░ Subject: Unit process exited
░░ Defined-By: systemd
░░ Support: https://access.redhat.com/support
░░ 
░░ An ExecStartPre= process belonging to unit mariadb.service has exited.
░░ 
░░ The process' exit code is 'exited' and its exit status is 0.
Jul 24 13:11:39 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Got f
inal SIGCHLD for state start-pre.
Jul 24 13:11:39 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Will 
spawn child (service_enter_start): /usr/libexec/mariadbd
Jul 24 13:11:39 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Passi
ng 0 fds to service
Jul 24 13:11:39 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: About
 to execute /usr/libexec/mariadbd --basedir=/usr "\$MYSQLD_OPTS" "\$_WSREP_NEW_CLUSTER"
Jul 24 13:11:39 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Forke
d /usr/libexec/mariadbd as 1463
Jul 24 13:11:39 stdb01.stratos.xfusioncorp.com systemd[1463]: PR_SET_MM_ARG_START
 failed: Operation not permitted
Jul 24 13:11:39 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Chang
ed start-pre -> start
Jul 24 13:11:39 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: User 
lookup succeeded: uid=27 gid=27
Jul 24 13:11:39 stdb01.stratos.xfusioncorp.com systemd[1463]: Bind-mounting / on 
/run/systemd/unit-root (MS_BIND|MS_REC "")...
Jul 24 13:11:39 stdb01.stratos.xfusioncorp.com systemd[1463]: Applying namespace 
mount on /run/systemd/unit-root/run/credentials
Jul 24 13:11:39 stdb01.stratos.xfusioncorp.com systemd[1463]: Bind-mounting /run/
systemd/inaccessible/dir on /run/systemd/unit-root/run/credentials (MS_BIND|MS_REC "")...
Jul 24 13:11:39 stdb01.stratos.xfusioncorp.com systemd[1463]: Successfully mounte
d /run/systemd/inaccessible/dir to /run/systemd/unit-root/run/credentials
Jul 24 13:11:39 stdb01.stratos.xfusioncorp.com systemd[1463]: Applying namespace 
mount on /run/systemd/unit-root/run/systemd/incoming
Jul 24 13:11:39 stdb01.stratos.xfusioncorp.com systemd[1463]: Followed source sym
links /run/systemd/propagate/mariadb.service → /run/systemd/propagate/mariadb.service.
Jul 24 13:11:39 stdb01.stratos.xfusioncorp.com systemd[1463]: Bind-mounting /run/
systemd/propagate/mariadb.service on /run/systemd/unit-root/run/systemd/incoming (MS_BIND "")...
Jul 24 13:11:39 stdb01.stratos.xfusioncorp.com systemd[1463]: Successfully mounte
d /run/systemd/propagate/mariadb.service to /run/systemd/unit-root/run/systemd/incoming
Jul 24 13:11:39 stdb01.stratos.xfusioncorp.com systemd[1463]: Applying namespace 
mount on /run/systemd/unit-root/tmp
Jul 24 13:11:39 stdb01.stratos.xfusioncorp.com systemd[1463]: Bind-mounting /tmp/
systemd-private-567bda43f04743f0b238c00f2a27c76d-mariadb.service-Ra7h8V/tmp on /run/systemd/unit-root/tmp (MS_BIN
D|MS_REC "")...
Jul 24 13:11:39 stdb01.stratos.xfusioncorp.com systemd[1463]: Successfully mounte
d /tmp/systemd-private-567bda43f04743f0b238c00f2a27c76d-mariadb.service-Ra7h8V/tmp to /run/systemd/unit-root/tmp

Jul 24 13:11:39 stdb01.stratos.xfusioncorp.com systemd[1463]: Applying namespace 
mount on /run/systemd/unit-root/var/tmp
Jul 24 13:11:39 stdb01.stratos.xfusioncorp.com systemd[1463]: Bind-mounting /var/
tmp/systemd-private-567bda43f04743f0b238c00f2a27c76d-mariadb.service-8hQDnj/tmp on /run/systemd/unit-root/var/tmp
 (MS_BIND|MS_REC "")...
Jul 24 13:11:39 stdb01.stratos.xfusioncorp.com systemd[1463]: Successfully mounte
d /var/tmp/systemd-private-567bda43f04743f0b238c00f2a27c76d-mariadb.service-8hQDnj/tmp to /run/systemd/unit-root/
var/tmp
Jul 24 13:11:39 stdb01.stratos.xfusioncorp.com systemd[1463]: Remounted /run/syst
emd/unit-root/run/credentials.
Jul 24 13:11:39 stdb01.stratos.xfusioncorp.com systemd[1463]: Remounted /run/syst
emd/unit-root/run/systemd/incoming.
Jul 24 13:11:39 stdb01.stratos.xfusioncorp.com systemd[1463]: Remounted /run/syst
emd/unit-root/run/credentials.
Jul 24 13:11:39 stdb01.stratos.xfusioncorp.com systemd[1463]: mariadb.service: Ex
ecuting: /usr/libexec/mariadbd --basedir=/usr
Jul 24 13:11:40 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Got n
otification message from PID 1463 (READY=1, STATUS=Taking your SQL requests now...)
Jul 24 13:11:40 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Will 
spawn child (service_enter_start_post): /usr/libexec/mariadb-check-upgrade
Jul 24 13:11:40 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: About
 to execute /usr/libexec/mariadb-check-upgrade
Jul 24 13:11:40 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Forke
d /usr/libexec/mariadb-check-upgrade as 1505
Jul 24 13:11:40 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Chang
ed start -> start-post
Jul 24 13:11:40 stdb01.stratos.xfusioncorp.com systemd[1505]: PR_SET_MM_ARG_START
 failed: Operation not permitted
Jul 24 13:11:40 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: User 
lookup succeeded: uid=27 gid=27
Jul 24 13:11:40 stdb01.stratos.xfusioncorp.com systemd[1505]: Bind-mounting / on 
/run/systemd/unit-root (MS_BIND|MS_REC "")...
Jul 24 13:11:40 stdb01.stratos.xfusioncorp.com systemd[1505]: Applying namespace 
mount on /run/systemd/unit-root/run/credentials
Jul 24 13:11:40 stdb01.stratos.xfusioncorp.com systemd[1505]: Bind-mounting /run/
systemd/inaccessible/dir on /run/systemd/unit-root/run/credentials (MS_BIND|MS_REC "")...
Jul 24 13:11:40 stdb01.stratos.xfusioncorp.com systemd[1505]: Successfully mounte
d /run/systemd/inaccessible/dir to /run/systemd/unit-root/run/credentials
Jul 24 13:11:40 stdb01.stratos.xfusioncorp.com systemd[1505]: Applying namespace 
mount on /run/systemd/unit-root/run/systemd/incoming
Jul 24 13:11:40 stdb01.stratos.xfusioncorp.com systemd[1505]: Followed source sym
links /run/systemd/propagate/mariadb.service → /run/systemd/propagate/mariadb.service.
Jul 24 13:11:40 stdb01.stratos.xfusioncorp.com systemd[1505]: Bind-mounting /run/
systemd/propagate/mariadb.service on /run/systemd/unit-root/run/systemd/incoming (MS_BIND "")...
Jul 24 13:11:40 stdb01.stratos.xfusioncorp.com systemd[1505]: Successfully mounte
d /run/systemd/propagate/mariadb.service to /run/systemd/unit-root/run/systemd/incoming
Jul 24 13:11:40 stdb01.stratos.xfusioncorp.com systemd[1505]: Applying namespace 
mount on /run/systemd/unit-root/tmp
Jul 24 13:11:40 stdb01.stratos.xfusioncorp.com systemd[1505]: Bind-mounting /tmp/
systemd-private-567bda43f04743f0b238c00f2a27c76d-mariadb.service-Ra7h8V/tmp on /run/systemd/unit-root/tmp (MS_BIN
D|MS_REC "")...
Jul 24 13:11:40 stdb01.stratos.xfusioncorp.com systemd[1505]: Successfully mounte
d /tmp/systemd-private-567bda43f04743f0b238c00f2a27c76d-mariadb.service-Ra7h8V/tmp to /run/systemd/unit-root/tmp

Jul 24 13:11:40 stdb01.stratos.xfusioncorp.com systemd[1505]: Applying namespace 
mount on /run/systemd/unit-root/var/tmp
Jul 24 13:11:40 stdb01.stratos.xfusioncorp.com systemd[1505]: Bind-mounting /var/
tmp/systemd-private-567bda43f04743f0b238c00f2a27c76d-mariadb.service-8hQDnj/tmp on /run/systemd/unit-root/var/tmp
 (MS_BIND|MS_REC "")...
Jul 24 13:11:40 stdb01.stratos.xfusioncorp.com systemd[1505]: Successfully mounte
d /var/tmp/systemd-private-567bda43f04743f0b238c00f2a27c76d-mariadb.service-8hQDnj/tmp to /run/systemd/unit-root/
var/tmp
Jul 24 13:11:40 stdb01.stratos.xfusioncorp.com systemd[1505]: Remounted /run/syst
emd/unit-root/run/credentials.
Jul 24 13:11:40 stdb01.stratos.xfusioncorp.com systemd[1505]: Remounted /run/syst
emd/unit-root/run/systemd/incoming.
Jul 24 13:11:40 stdb01.stratos.xfusioncorp.com systemd[1505]: Remounted /run/syst
emd/unit-root/run/credentials.
Jul 24 13:11:40 stdb01.stratos.xfusioncorp.com systemd[1505]: mariadb.service: Ex
ecuting: /usr/libexec/mariadb-check-upgrade
Jul 24 13:11:40 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Child
 1505 belongs to mariadb.service.
Jul 24 13:11:40 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Contr
ol process exited, code=exited, status=0/SUCCESS (success)
░░ Subject: Unit process exited
░░ Defined-By: systemd
░░ Support: https://access.redhat.com/support
░░ 
░░ An ExecStartPost= process belonging to unit mariadb.service has exited.
░░ 
░░ The process' exit code is 'exited' and its exit status is 0.
Jul 24 13:11:40 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Got f
inal SIGCHLD for state start-post.
Jul 24 13:11:40 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Chang
ed start-post -> running
Jul 24 13:11:40 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Job 5
9 mariadb.service/start finished, result=done
Jul 24 13:11:40 stdb01.stratos.xfusioncorp.com systemd[1]: Started MariaDB 10.5 database server.
░░ Subject: A start job for unit mariadb.service has finished successfully
░░ Defined-By: systemd
░░ Support: https://access.redhat.com/support
░░ 
░░ A start job for unit mariadb.service has finished successfully.
░░ 
░░ The job identifier is 59.
Jul 24 13:11:46 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Tryin
g to enqueue job mariadb.service/stop/replace
Jul 24 13:11:46 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Insta
lled new job mariadb.service/stop as 114
Jul 24 13:11:46 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Enque
ued job mariadb.service/stop as 114
Jul 24 13:11:46 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Chang
ed running -> stop-sigterm
Jul 24 13:11:46 stdb01.stratos.xfusioncorp.com systemd[1]: Stopping MariaDB 10.5 database server...
░░ Subject: A stop job for unit mariadb.service has begun execution
░░ Defined-By: systemd
░░ Support: https://access.redhat.com/support
░░ 
░░ A stop job for unit mariadb.service has begun execution.
░░ 
░░ The job identifier is 114.
Jul 24 13:11:46 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Got n
otification message from PID 1463 (STOPPING=1, STATUS=Shutdown in progress)
Jul 24 13:11:46 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Got n
otification message from PID 1463 (STATUS=Dumping buffer pool page 1/176, EXTEND_TIMEOUT_USEC=30000000)
Jul 24 13:11:46 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Got n
otification message from PID 1463 (STATUS=Waiting for page cleaner thread to exit, EXTEND_TIMEOUT_USEC=120000000)

Jul 24 13:11:46 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Got n
otification message from PID 1463 (STATUS=Waiting to flush the buffer pool, EXTEND_TIMEOUT_USEC=30000000)
Jul 24 13:11:46 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Got n
otification message from PID 1463 (STATUS=ensuring dirty buffer pool are written to log, EXTEND_TIMEOUT_USEC=3000
0000)
Jul 24 13:11:46 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Got n
otification message from PID 1463 (STATUS=Free innodb buffer pool, EXTEND_TIMEOUT_USEC=30000000)
Jul 24 13:11:46 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Got n
otification message from PID 1463 (STATUS=MariaDB server is down)
Jul 24 13:11:46 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Got n
otification message from PID 1463 (STATUS=MariaDB server is down)
Jul 24 13:11:46 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Child
 1463 belongs to mariadb.service.
Jul 24 13:11:46 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Main 
process exited, code=exited, status=0/SUCCESS (success)
░░ Subject: Unit process exited
░░ Defined-By: systemd
░░ Support: https://access.redhat.com/support
░░ 
░░ An ExecStart= process belonging to unit mariadb.service has exited.
░░ 
░░ The process' exit code is 'exited' and its exit status is 0.
Jul 24 13:11:46 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Deactivated successfully.
░░ Subject: Unit succeeded
░░ Defined-By: systemd
░░ Support: https://access.redhat.com/support
░░ 
░░ The unit mariadb.service has successfully entered the 'dead' state.
Jul 24 13:11:46 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Servi
ce restart not allowed.
Jul 24 13:11:46 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Chang
ed stop-sigterm -> dead
Jul 24 13:11:46 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Job 1
14 mariadb.service/stop finished, result=done
Jul 24 13:11:46 stdb01.stratos.xfusioncorp.com systemd[1]: Stopped MariaDB 10.5 database server.
░░ Subject: A stop job for unit mariadb.service has finished
░░ Defined-By: systemd
░░ Support: https://access.redhat.com/support
░░ 
░░ A stop job for unit mariadb.service has finished.
░░ 
░░ The job identifier is 114 and the job result is done.
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Tryin
g to enqueue job mariadb.service/restart/replace
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Insta
lled new job mariadb.service/restart as 243
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Enque
ued job mariadb.service/restart as 243
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Job 2
43 mariadb.service/restart finished, result=done
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Conve
rting job mariadb.service/restart -> mariadb.service/start
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Will 
spawn child (service_enter_start_pre): /usr/libexec/mariadb-check-socket
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Faile
d to reset devices.allow/devices.deny: Operation not permitted
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Faile
d to set 'trusted.invocation_id' xattr on control group /docker/f77b6d5bd6c2b7a41a47d55dd28048af36287f661f148fbe5
4d8ae6677e0ab48/system.slice/mariadb.service, ignoring: Operation not permitted
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Faile
d to remove 'trusted.delegate' xattr flag on control group /docker/f77b6d5bd6c2b7a41a47d55dd28048af36287f661f148f
be54d8ae6677e0ab48/system.slice/mariadb.service, ignoring: Operation not permitted
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: About
 to execute /usr/libexec/mariadb-check-socket
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Forke
d /usr/libexec/mariadb-check-socket as 2504
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[2504]: PR_SET_MM_ARG_START
 failed: Operation not permitted
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Chang
ed dead -> start-pre
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[1]: Starting MariaDB 10.5 database server...
░░ Subject: A start job for unit mariadb.service has begun execution
░░ Defined-By: systemd
░░ Support: https://access.redhat.com/support
░░ 
░░ A start job for unit mariadb.service has begun execution.
░░ ore--
░░ The job identifier is 243.
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: User 
lookup succeeded: uid=27 gid=27
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[2504]: Bind-mounting / on 
/run/systemd/unit-root (MS_BIND|MS_REC "")...
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[2504]: Applying namespace 
mount on /run/systemd/unit-root/run/credentials
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[2504]: Bind-mounting /run/
systemd/inaccessible/dir on /run/systemd/unit-root/run/credentials (MS_BIND|MS_REC "")...
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[2504]: Successfully mounte
d /run/systemd/inaccessible/dir to /run/systemd/unit-root/run/credentials
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[2504]: Applying namespace 
mount on /run/systemd/unit-root/run/systemd/incoming
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[2504]: Followed source sym
links /run/systemd/propagate/mariadb.service → /run/systemd/propagate/mariadb.service.
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[2504]: Bind-mounting /run/
systemd/propagate/mariadb.service on /run/systemd/unit-root/run/systemd/incoming (MS_BIND "")...
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[2504]: Successfully mounte
d /run/systemd/propagate/mariadb.service to /run/systemd/unit-root/run/systemd/incoming
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[2504]: Applying namespace 
mount on /run/systemd/unit-root/tmp
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[2504]: Bind-mounting /tmp/
systemd-private-567bda43f04743f0b238c00f2a27c76d-mariadb.service-2xbm7T/tmp on /run/systemd/unit-root/tmp (MS_BIN
D|MS_REC "")...
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[2504]: Successfully mounte
d /tmp/systemd-private-567bda43f04743f0b238c00f2a27c76d-mariadb.service-2xbm7T/tmp to /run/systemd/unit-root/tmp

Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[2504]: Applying namespace 
mount on /run/systemd/unit-root/var/tmp
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[2504]: Bind-mounting /var/
tmp/systemd-private-567bda43f04743f0b238c00f2a27c76d-mariadb.service-uvOIQr/tmp on /run/systemd/unit-root/var/tmp
 (MS_BIND|MS_REC "")...
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[2504]: Successfully mounte
d /var/tmp/systemd-private-567bda43f04743f0b238c00f2a27c76d-mariadb.service-uvOIQr/tmp to /run/systemd/unit-root/
var/tmp
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[2504]: Remounted /run/syst
emd/unit-root/run/credentials.
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[2504]: Remounted /run/syst
emd/unit-root/run/systemd/incoming.
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[2504]: Remounted /run/syst
emd/unit-root/run/credentials.
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[2504]: mariadb.service: Ex
ecuting: /usr/libexec/mariadb-check-socket
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Child
 2504 belongs to mariadb.service.
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Contr
ol process exited, code=exited, status=0/SUCCESS (success)
░░ Subject: Unit process exited
░░ Defined-By: systemd
░░ Support: https://access.redhat.com/support
░░ 
░░ An ExecStartPre= process belonging to unit mariadb.service has exited.
░░ 
░░ The process' exit code is 'exited' and its exit status is 0.
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Runni
ng next control command for state start-pre.
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Will 
spawn child (service_run_next_control): /usr/libexec/mariadb-prepare-db-dir
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: About
 to execute /usr/libexec/mariadb-prepare-db-dir mariadb.service
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Forke
d /usr/libexec/mariadb-prepare-db-dir as 2537
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[2537]: PR_SET_MM_ARG_START
 failed: Operation not permitted
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: User 
lookup succeeded: uid=27 gid=27
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[2537]: Bind-mounting / on 
/run/systemd/unit-root (MS_BIND|MS_REC "")...
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[2537]: Applying namespace 
mount on /run/systemd/unit-root/run/credentials
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[2537]: Bind-mounting /run/
systemd/inaccessible/dir on /run/systemd/unit-root/run/credentials (MS_BIND|MS_REC "")...
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[2537]: Successfully mounte
d /run/systemd/inaccessible/dir to /run/systemd/unit-root/run/credentials
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[2537]: Applying namespace 
mount on /run/systemd/unit-root/run/systemd/incoming
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[2537]: Followed source sym
links /run/systemd/propagate/mariadb.service → /run/systemd/propagate/mariadb.service.
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[2537]: Bind-mounting /run/
systemd/propagate/mariadb.service on /run/systemd/unit-root/run/systemd/incoming (MS_BIND "")...
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[2537]: Successfully mounte
d /run/systemd/propagate/mariadb.service to /run/systemd/unit-root/run/systemd/incoming
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[2537]: Applying namespace 
mount on /run/systemd/unit-root/tmp
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[2537]: Bind-mounting /tmp/
systemd-private-567bda43f04743f0b238c00f2a27c76d-mariadb.service-2xbm7T/tmp on /run/systemd/unit-root/tmp (MS_BIN
D|MS_REC "")...
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[2537]: Successfully mounte
d /tmp/systemd-private-567bda43f04743f0b238c00f2a27c76d-mariadb.service-2xbm7T/tmp to /run/systemd/unit-root/tmp

Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[2537]: Applying namespace 
mount on /run/systemd/unit-root/var/tmp
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[2537]: Bind-mounting /var/
tmp/systemd-private-567bda43f04743f0b238c00f2a27c76d-mariadb.service-uvOIQr/tmp on /run/systemd/unit-root/var/tmp
 (MS_BIND|MS_REC "")...
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[2537]: Successfully mounte
d /var/tmp/systemd-private-567bda43f04743f0b238c00f2a27c76d-mariadb.service-uvOIQr/tmp to /run/systemd/unit-root/
var/tmp
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[2537]: Remounted /run/syst
emd/unit-root/run/credentials.
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[2537]: Remounted /run/syst
emd/unit-root/run/systemd/incoming.
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[2537]: Remounted /run/syst
emd/unit-root/run/credentials.
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[2537]: mariadb.service: Ex
ecuting: /usr/libexec/mariadb-prepare-db-dir mariadb.service
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com mariadb-prepare-db-dir[2537]: Database MariaDB is probably initial
ized in /var/lib/mysql already, nothing is done.
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com mariadb-prepare-db-dir[2537]: If this is not the case, make sure t
he /var/lib/mysql is empty before running mariadb-prepare-db-dir.
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Child
 2537 belongs to mariadb.service.
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Contr
ol process exited, code=exited, status=0/SUCCESS (success)
░░ Subject: Unit process exited
░░ Defined-By: systemd
░░ Support: https://access.redhat.com/support
░░ 
░░ An ExecStartPre= process belonging to unit mariadb.service has exited.
░░ 
░░ The process' exit code is 'exited' and its exit status is 0.
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Got f
inal SIGCHLD for state start-pre.
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Will 
spawn child (service_enter_start): /usr/libexec/mariadbd
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Passi
ng 0 fds to service
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: About
 to execute /usr/libexec/mariadbd --basedir=/usr "\$MYSQLD_OPTS" "\$_WSREP_NEW_CLUSTER"
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Forke
d /usr/libexec/mariadbd as 2632
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Chang
ed start-pre -> start
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[2632]: PR_SET_MM_ARG_START
 failed: Operation not permitted
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: User 
lookup succeeded: uid=27 gid=27
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[2632]: Bind-mounting / on 
/run/systemd/unit-root (MS_BIND|MS_REC "")...
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[2632]: Applying namespace 
mount on /run/systemd/unit-root/run/credentials
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[2632]: Bind-mounting /run/
systemd/inaccessible/dir on /run/systemd/unit-root/run/credentials (MS_BIND|MS_REC "")...
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[2632]: Successfully mounte
d /run/systemd/inaccessible/dir to /run/systemd/unit-root/run/credentials
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[2632]: Applying namespace 
mount on /run/systemd/unit-root/run/systemd/incoming
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[2632]: Followed source sym
links /run/systemd/propagate/mariadb.service → /run/systemd/propagate/mariadb.service.
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[2632]: Bind-mounting /run/
systemd/propagate/mariadb.service on /run/systemd/unit-root/run/systemd/incoming (MS_BIND "")...
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[2632]: Successfully mounte
d /run/systemd/propagate/mariadb.service to /run/systemd/unit-root/run/systemd/incoming
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[2632]: Applying namespace 
mount on /run/systemd/unit-root/tmp
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[2632]: Bind-mounting /tmp/
systemd-private-567bda43f04743f0b238c00f2a27c76d-mariadb.service-2xbm7T/tmp on /run/systemd/unit-root/tmp (MS_BIN
D|MS_REC "")...
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[2632]: Successfully mounte
d /tmp/systemd-private-567bda43f04743f0b238c00f2a27c76d-mariadb.service-2xbm7T/tmp to /run/systemd/unit-root/tmp

Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[2632]: Applying namespace 
mount on /run/systemd/unit-root/var/tmp
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[2632]: Bind-mounting /var/
tmp/systemd-private-567bda43f04743f0b238c00f2a27c76d-mariadb.service-uvOIQr/tmp on /run/systemd/unit-root/var/tmp
 (MS_BIND|MS_REC "")...
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[2632]: Successfully mounte
d /var/tmp/systemd-private-567bda43f04743f0b238c00f2a27c76d-mariadb.service-uvOIQr/tmp to /run/systemd/unit-root/
var/tmp
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[2632]: Remounted /run/syst
emd/unit-root/run/credentials.
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[2632]: Remounted /run/syst
emd/unit-root/run/systemd/incoming.
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[2632]: Remounted /run/syst
emd/unit-root/run/credentials.
Jul 24 13:13:43 stdb01.stratos.xfusioncorp.com systemd[2632]: mariadb.service: Ex
ecuting: /usr/libexec/mariadbd --basedir=/usr
Jul 24 13:13:44 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Child
 2632 belongs to mariadb.service.
Jul 24 13:13:44 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Main process 
exited, code=exited, status=1/FAILURE
░░ Subject: Unit process exited
░░ Defined-By: systemd
░░ Support: https://access.redhat.com/support
[peter@stdb01 ~]$ ls /var/log/mariadb/
ls: cannot open directory '/var/log/mariadb/': Permission denied
[peter@stdb01 ~]$ sudo ls /var/log/mariadb/
mariadb.log
[peter@stdb01 ~]$ sudo less /var/log/mariadb/mariadb.log
sudo: less: command not found
[peter@stdb01 ~]$ sudo cat /var/log/mariadb/mariadb.log
2025-07-24 13:11:39 0 [Note] Starting MariaDB 10.5.22-MariaDB source revision 7e650253dc488debcb0898ebe6d385bf6dfa3656 as process 1463
2025-07-24 13:11:40 0 [Note] InnoDB: Uses event mutexes
2025-07-24 13:11:40 0 [Note] InnoDB: Compressed tables use zlib 1.2.11
2025-07-24 13:11:40 0 [Note] InnoDB: Number of pools: 1
2025-07-24 13:11:40 0 [Note] InnoDB: Using crc32 + pclmulqdq instructions
2025-07-24 13:11:40 0 [Note] mariadbd: O_TMPFILE is not supported on /var/tmp (disabling future attempts)
2025-07-24 13:11:40 0 [Note] InnoDB: Using Linux native AIO
2025-07-24 13:11:40 0 [Note] InnoDB: Initializing buffer pool, total size = 134217728, chunk size = 134217728
2025-07-24 13:11:40 0 [Note] InnoDB: Completed initialization of buffer pool
2025-07-24 13:11:40 0 [Note] InnoDB: 128 rollback segments are active.
2025-07-24 13:11:40 0 [Note] InnoDB: Creating shared tablespace for temporary tables
2025-07-24 13:11:40 0 [Note] InnoDB: Setting file './ibtmp1' size to 12 MB. Physically writing the file full; Please wait ...
2025-07-24 13:11:40 0 [Note] InnoDB: File './ibtmp1' size is now 12 MB.
2025-07-24 13:11:40 0 [Note] InnoDB: 10.5.22 started; log sequence number 45079; transaction id 20
2025-07-24 13:11:40 0 [Note] Plugin 'FEEDBACK' is disabled.
2025-07-24 13:11:40 0 [Note] InnoDB: Loading buffer pool(s) from /var/lib/mysql/ib_buffer_pool
2025-07-24 13:11:40 0 [Note] Server socket created on IP: '::'.
2025-07-24 13:11:40 0 [Note] InnoDB: Buffer pool(s) load completed at 250724 13:11:40
2025-07-24 13:11:40 0 [Note] Reading of all Master_info entries succeeded
2025-07-24 13:11:40 0 [Note] Added new Master_info '' to hash table
2025-07-24 13:11:40 0 [Note] /usr/libexec/mariadbd: ready for connections.
Version: '10.5.22-MariaDB'  socket: '/var/lib/mysql/mysql.sock'  port: 3306  MariaDB Server
2025-07-24 13:11:46 0 [Note] /usr/libexec/mariadbd (initiated by: unknown): Normal shutdown
2025-07-24 13:11:46 0 [Note] Event Scheduler: Purging the queue. 0 events
2025-07-24 13:11:46 0 [Note] InnoDB: FTS optimize thread exiting.
2025-07-24 13:11:46 0 [Note] InnoDB: Starting shutdown...
2025-07-24 13:11:46 0 [Note] InnoDB: Dumping buffer pool(s) to /var/lib/mysql/ib_buffer_pool
2025-07-24 13:11:46 0 [Note] InnoDB: Buffer pool(s) dump completed at 250724 13:11:46
2025-07-24 13:11:46 0 [Note] InnoDB: Removed temporary tablespace data file: "ibtmp1"
2025-07-24 13:11:46 0 [Note] InnoDB: Shutdown completed; log sequence number 45091; transaction id 21
2025-07-24 13:11:46 0 [Note] /usr/libexec/mariadbd: Shutdown complete

2025-07-24 13:13:43 0 [Note] Starting MariaDB 10.5.22-MariaDB source revision 7e650253dc488debcb0898ebe6d385bf6dfa3656 as process 2632
2025-07-24 13:13:43 0 [Note] InnoDB: Uses event mutexes
2025-07-24 13:13:43 0 [Note] InnoDB: Compressed tables use zlib 1.2.11
2025-07-24 13:13:43 0 [Note] InnoDB: Number of pools: 1
2025-07-24 13:13:43 0 [Note] InnoDB: Using crc32 + pclmulqdq instructions
2025-07-24 13:13:43 0 [Note] mariadbd: O_TMPFILE is not supported on /var/tmp (disabling future attempts)
2025-07-24 13:13:44 0 [Note] InnoDB: Using Linux native AIO
2025-07-24 13:13:44 0 [Note] InnoDB: Initializing buffer pool, total size = 134217728, chunk size = 134217728
2025-07-24 13:13:44 0 [Note] InnoDB: Completed initialization of buffer pool
2025-07-24 13:13:44 0 [Note] InnoDB: 128 rollback segments are active.
2025-07-24 13:13:44 0 [Note] InnoDB: Creating shared tablespace for temporary tables
2025-07-24 13:13:44 0 [Note] InnoDB: Setting file './ibtmp1' size to 12 MB. Physically writing the file full; Please wait ...
2025-07-24 13:13:44 0 [Note] InnoDB: File './ibtmp1' size is now 12 MB.
2025-07-24 13:13:44 0 [Note] InnoDB: 10.5.22 started; log sequence number 45091; transaction id 20
2025-07-24 13:13:44 0 [Note] InnoDB: Loading buffer pool(s) from /var/lib/mysql/ib_buffer_pool
2025-07-24 13:13:44 0 [Note] Plugin 'FEEDBACK' is disabled.
2025-07-24 13:13:44 0 [Note] Server socket created on IP: '::'.
2025-07-24 13:13:44 0 [ERROR] mariadbd: Can't create/write to file '/run/mariadb/mariadb.pid' (Errcode: 13 "Permission denied")
2025-07-24 13:13:44 0 [ERROR] Can't start server: can't create PID file: Permission denied
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
