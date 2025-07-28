# Create a Cron Job

The `Nautilus` system admins team has prepared scripts to automate several day-to-day tasks. They want them to be deployed on all app servers in `Stratos DC` on a set schedule. Before that they need to test similar functionality with a sample cron job. Therefore, perform the steps below:

a. Install `cronie` package on all `Nautilus` app servers and start `crond` service.

b. Add a cron `*/5 * * * * echo hello > /tmp/cron_text` for `root` user.



My Solution:

```
thor@jumphost ~$ ssh tony@172.16.238.10
The authenticity of host '172.16.238.10 (172.16.238.10)' can't be established.
ED25519 key fingerprint is SHA256:yiBAQ3Vl+i2jpfDqE2Lccp1Qe5ZQLCc/Xx2Nem/7orQ.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '172.16.238.10' (ED25519) to the list of known hosts.
tony@172.16.238.10's password: 
[tony@stapp01 ~]$ sudo dnf install cronie -y

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for tony: 
CentOS Stream 9 - BaseOS                                                     28 kB/s | 6.7 kB     00:00    
CentOS Stream 9 - BaseOS                                                     18 MB/s | 8.8 MB     00:00    
CentOS Stream 9 - AppStream                                                  31 kB/s | 6.8 kB     00:00    
CentOS Stream 9 - AppStream                                                  14 MB/s |  24 MB     00:01    
CentOS Stream 9 - Extras packages                                            34 kB/s | 7.3 kB     00:00    
CentOS Stream 9 - Extras packages                                            75 kB/s |  19 kB     00:00    
Extra Packages for Enterprise Linux 9 - x86_64                              158 kB/s |  34 kB     00:00    
Extra Packages for Enterprise Linux 9 - x86_64                               17 MB/s |  20 MB     00:01    
Extra Packages for Enterprise Linux 9 openh264 (From Cisco) - x86_64        2.9 kB/s | 993  B     00:00    
Extra Packages for Enterprise Linux 9 - Next - x86_64                       102 kB/s |  24 kB     00:00    
Extra Packages for Enterprise Linux 9 - Next - x86_64                       150 kB/s | 120 kB     00:00    
Dependencies resolved.
============================================================================================================
 Package                      Architecture         Version                       Repository            Size
============================================================================================================
Installing:
 cronie                       x86_64               1.5.7-14.el9                  baseos               118 k
Installing dependencies:
 cronie-anacron               x86_64               1.5.7-14.el9                  baseos                32 k

Transaction Summary
============================================================================================================
Install  2 Packages

Total download size: 150 k
Installed size: 346 k
Downloading Packages:
(1/2): cronie-anacron-1.5.7-14.el9.x86_64.rpm                               142 kB/s |  32 kB     00:00    
(2/2): cronie-1.5.7-14.el9.x86_64.rpm                                       385 kB/s | 118 kB     00:00    
------------------------------------------------------------------------------------------------------------
Total                                                                       283 kB/s | 150 kB     00:00     
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                                                    1/1 
  Installing       : cronie-anacron-1.5.7-14.el9.x86_64                                                 1/2 
  Running scriptlet: cronie-anacron-1.5.7-14.el9.x86_64                                                 1/2 
  Installing       : cronie-1.5.7-14.el9.x86_64                                                         2/2 
  Running scriptlet: cronie-1.5.7-14.el9.x86_64                                                         2/2 
Created symlink /etc/systemd/system/multi-user.target.wants/crond.service → /usr/lib/systemd/system/crond.service.

  Verifying        : cronie-1.5.7-14.el9.x86_64                                                         1/2 
  Verifying        : cronie-anacron-1.5.7-14.el9.x86_64                                                 2/2 

Installed:
  cronie-1.5.7-14.el9.x86_64                       cronie-anacron-1.5.7-14.el9.x86_64                      

Complete!
[tony@stapp01 ~]$ sudo systemctl enable --now crond
[tony@stapp01 ~]$ sudo crontab -u root -e 
no crontab for root - using an empty one
crontab: installing new crontab
[tony@stapp01 ~]$ exit
logout
Connection to 172.16.238.10 closed.
thor@jumphost ~$ ssh steven@172.16.238.11
The authenticity of host '172.16.238.11 (172.16.238.11)' can't be established.
ED25519 key fingerprint is SHA256:C6zRp0JEAZPstaZjIWKs4jQy9QQGQTUM18PO9qEjirw.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '172.16.238.11' (ED25519) to the list of known hosts.
steven@172.16.238.11's password: 
Permission denied, please try again.
steven@172.16.238.11's password: 
Permission denied, please try again.
steven@172.16.238.11's password: 
steven@172.16.238.11: Permission denied (publickey,gssapi-keyex,gssapi-with-mic,password).
thor@jumphost ~$ ssh steven@172.16.238.11
steven@172.16.238.11's password: 
Permission denied, please try again.
steven@172.16.238.11's password: 
Permission denied, please try again.
steven@172.16.238.11's password: 
steven@172.16.238.11: Permission denied (publickey,gssapi-keyex,gssapi-with-mic,password).
thor@jumphost ~$ ssh steve@172.16.238.11
steve@172.16.238.11's password: 
[steve@stapp02 ~]$ sudo dnf install cronie -y

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for steve: 
CentOS Stream 9 - BaseOS                                                     35 kB/s | 6.7 kB     00:00    
CentOS Stream 9 - BaseOS                                                     22 MB/s | 8.8 MB     00:00    
CentOS Stream 9 - AppStream                                                  48 kB/s | 6.8 kB     00:00    
CentOS Stream 9 - AppStream                                                  25 MB/s |  24 MB     00:00    
CentOS Stream 9 - Extras packages                                            45 kB/s | 7.3 kB     00:00    
CentOS Stream 9 - Extras packages                                            29 kB/s |  19 kB     00:00    
Extra Packages for Enterprise Linux 9 - x86_64                              120 kB/s |  34 kB     00:00    
Extra Packages for Enterprise Linux 9 - x86_64                               19 MB/s |  20 MB     00:01    
Extra Packages for Enterprise Linux 9 openh264 (From Cisco) - x86_64        7.0 kB/s | 993  B     00:00    
Extra Packages for Enterprise Linux 9 - Next - x86_64                        92 kB/s |  24 kB     00:00    
Extra Packages for Enterprise Linux 9 - Next - x86_64                       253 kB/s | 120 kB     00:00    
Dependencies resolved.
============================================================================================================
 Package                      Architecture         Version                       Repository            Size
============================================================================================================
Installing:
 cronie                       x86_64               1.5.7-14.el9                  baseos               118 k
Installing dependencies:
 cronie-anacron               x86_64               1.5.7-14.el9                  baseos                32 k

Transaction Summary
============================================================================================================
Install  2 Packages

Total download size: 150 k
Installed size: 346 k
Downloading Packages:
(1/2): cronie-anacron-1.5.7-14.el9.x86_64.rpm                               811 kB/s |  32 kB     00:00    
(2/2): cronie-1.5.7-14.el9.x86_64.rpm                                       2.2 MB/s | 118 kB     00:00    
------------------------------------------------------------------------------------------------------------
Total                                                                       409 kB/s | 150 kB     00:00     
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                                                    1/1 
  Installing       : cronie-anacron-1.5.7-14.el9.x86_64                                                 1/2 
  Running scriptlet: cronie-anacron-1.5.7-14.el9.x86_64                                                 1/2 
  Installing       : cronie-1.5.7-14.el9.x86_64                                                         2/2 
  Running scriptlet: cronie-1.5.7-14.el9.x86_64                                                         2/2 
Created symlink /etc/systemd/system/multi-user.target.wants/crond.service → /usr/lib/systemd/system/crond.service.

  Verifying        : cronie-1.5.7-14.el9.x86_64                                                         1/2 
  Verifying        : cronie-anacron-1.5.7-14.el9.x86_64                                                 2/2 

Installed:
  cronie-1.5.7-14.el9.x86_64                       cronie-anacron-1.5.7-14.el9.x86_64                      

Complete!
[steve@stapp02 ~]$ sudo systemctl enable --now crond
[steve@stapp02 ~]$ sudo crontab -u root -e
no crontab for root - using an empty one
crontab: installing new crontab
[steve@stapp02 ~]$ exit
logout
Connection to 172.16.238.11 closed.
thor@jumphost ~$ ssh banner@172.16.238.12
The authenticity of host '172.16.238.12 (172.16.238.12)' can't be established.
ED25519 key fingerprint is SHA256:cjZy63wuoFayzI3yrb218LjWJmAbe7N/bOXv9j/Z9ko.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '172.16.238.12' (ED25519) to the list of known hosts.
banner@172.16.238.12's password: 
[banner@stapp03 ~]$ sudo dnf install cronie -y

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for banner: 
Sorry, try again.
[sudo] password for banner: 
CentOS Stream 9 - BaseOS                                                     34 kB/s | 6.7 kB     00:00    
CentOS Stream 9 - BaseOS                                                     10 MB/s | 8.8 MB     00:00    
CentOS Stream 9 - AppStream                                                  38 kB/s | 6.8 kB     00:00    
CentOS Stream 9 - AppStream                                                  24 MB/s |  24 MB     00:01    
CentOS Stream 9 - Extras packages                                            38 kB/s | 7.3 kB     00:00    
CentOS Stream 9 - Extras packages                                            47 kB/s |  19 kB     00:00    
Extra Packages for Enterprise Linux 9 - x86_64                               99 kB/s |  34 kB     00:00    
Extra Packages for Enterprise Linux 9 - x86_64                               12 MB/s |  20 MB     00:01    
Extra Packages for Enterprise Linux 9 openh264 (From Cisco) - x86_64        4.6 kB/s | 993  B     00:00    
Extra Packages for Enterprise Linux 9 - Next - x86_64                        95 kB/s |  24 kB     00:00    
Extra Packages for Enterprise Linux 9 - Next - x86_64                       122 kB/s | 120 kB     00:00    
Dependencies resolved.
============================================================================================================
 Package                      Architecture         Version                       Repository            Size
============================================================================================================
Installing:
 cronie                       x86_64               1.5.7-14.el9                  baseos               118 k
Installing dependencies:
 cronie-anacron               x86_64               1.5.7-14.el9                  baseos                32 k

Transaction Summary
============================================================================================================
Install  2 Packages

Total download size: 150 k
Installed size: 346 k
Downloading Packages:
(1/2): cronie-anacron-1.5.7-14.el9.x86_64.rpm                               274 kB/s |  32 kB     00:00    
(2/2): cronie-1.5.7-14.el9.x86_64.rpm                                       513 kB/s | 118 kB     00:00    
------------------------------------------------------------------------------------------------------------
Total                                                                       349 kB/s | 150 kB     00:00     
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                                                    1/1 
  Installing       : cronie-anacron-1.5.7-14.el9.x86_64                                                 1/2 
  Running scriptlet: cronie-anacron-1.5.7-14.el9.x86_64                                                 1/2 
  Installing       : cronie-1.5.7-14.el9.x86_64                                                         2/2 
  Running scriptlet: cronie-1.5.7-14.el9.x86_64                                                         2/2 
Created symlink /etc/systemd/system/multi-user.target.wants/crond.service → /usr/lib/systemd/system/crond.service.

  Verifying        : cronie-1.5.7-14.el9.x86_64                                                         1/2 
  Verifying        : cronie-anacron-1.5.7-14.el9.x86_64                                                 2/2 

Installed:
  cronie-1.5.7-14.el9.x86_64                       cronie-anacron-1.5.7-14.el9.x86_64                      

Complete!
[banner@stapp03 ~]$ sudo systemctl enable --now crond
[banner@stapp03 ~]$ sudo crontab -u -e
crontab:  user `-e' unknown
[banner@stapp03 ~]$ sudo crontab -u root -e
no crontab for root - using an empty one
crontab: installing new crontab
[banner@stapp03 ~]$ 
```
