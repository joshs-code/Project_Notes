# Restrict Cron Access

In alignment with security compliance standards, the Nautilus project team has opted to impose restrictions on crontab access. Specifically, only designated users will be permitted to create or update cron jobs.

Configure crontab access on App Server 1 as follows: Allow crontab access to `john` user while denying access to the `garrett` user.



My Solution:

```
thor@jumphost ~$ ssh tony@172.16.238.10
The authenticity of host '172.16.238.10 (172.16.238.10)' can't be established.
ED25519 key fingerprint is SHA256:BVVkyK6hkpgkORYZSrZfp4/VRDfcOplfxRCkGwrBuNk.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '172.16.238.10' (ED25519) to the list of known hosts.
tony@172.16.238.10's password: 
[tony@stapp01 ~]$ ls /etc/cron.d
cron.d/     cron.daily/ 
[tony@stapp01 ~]$ ls /etc/cron.d
cron.d/     cron.daily/ 
[tony@stapp01 ~]$ ls /etc/cron.d
0hourly
[tony@stapp01 ~]$ sudo vi /etc/cron.allow

===== cron.allow =====
john
==== end cron.allow ====

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for tony: 
[tony@stapp01 ~]$ sudo vi /etc/cron.deny

===== cron.deny =====
garrett
==== end cron.deny ====

[tony@stapp01 ~]$ sudo systemctl restart crond
```
