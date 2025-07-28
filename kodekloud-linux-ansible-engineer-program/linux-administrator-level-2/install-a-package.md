# Install a package

As per new application requirements shared by the `Nautilus` project development team, serveral new packages need to be installed on all app servers in `Stratos Datacenter`. Most of them are completed except for `epel-release`.

Therefore, install the `epel-release` package on all `app-servers`.



My Solution:

```
thor@jumphost ~$ ssh tony@172.16.238.10
The authenticity of host '172.16.238.10 (172.16.238.10)' can't be established.
ED25519 key fingerprint is SHA256:4zLdKzkHT23y1lE2M/lYIA3MrxreDrXksi3uhWdUjgc.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '172.16.238.10' (ED25519) to the list of known hosts.
tony@172.16.238.10's password: 
[tony@stapp01 ~]$ sudo dnf install epel-release -y

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for tony: 
CentOS Stream 9 - BaseOS                                                     30 kB/s | 6.7 kB     00:00    
CentOS Stream 9 - BaseOS                                                    8.3 MB/s | 8.8 MB     00:01    
CentOS Stream 9 - AppStream                                                  21 kB/s | 6.8 kB     00:00    
CentOS Stream 9 - AppStream                                                 4.7 MB/s |  24 MB     00:05    
CentOS Stream 9 - Extras packages                                            28 kB/s | 7.3 kB     00:00    
CentOS Stream 9 - Extras packages                                            33 kB/s |  19 kB     00:00    
Last metadata expiration check: 0:00:01 ago on Wed Jul 16 13:48:52 2025.
Dependencies resolved.
============================================================================================================
 Package                           Architecture    Version                     Repository              Size
============================================================================================================
Installing:
 epel-release                      noarch          9-7.el9                     extras-common           19 k
Installing dependencies:
 dbus-libs                         x86_64          1:1.12.20-8.el9             baseos                 152 k
 python3-dateutil                  noarch          1:2.8.1-7.el9               baseos                 288 k
 python3-dbus                      x86_64          1.2.18-2.el9                baseos                 144 k
 python3-dnf-plugins-core          noarch          4.3.0-21.el9                baseos                 264 k
 python3-six                       noarch          1.15.0-9.el9                baseos                  37 k
 python3-systemd                   x86_64          234-19.el9                  baseos                  89 k
Installing weak dependencies:
 dnf-plugins-core                  noarch          4.3.0-21.el9                baseos                  37 k
 epel-next-release                 noarch          9-7.el9                     extras-common          8.1 k

Transaction Summary
============================================================================================================
Install  9 Packages

Total download size: 1.0 M
Installed size: 3.0 M
Downloading Packages:
(1/9): dnf-plugins-core-4.3.0-21.el9.noarch.rpm                             226 kB/s |  37 kB     00:00    
(2/9): python3-dbus-1.2.18-2.el9.x86_64.rpm                                 1.0 MB/s | 144 kB     00:00    
(3/9): dbus-libs-1.12.20-8.el9.x86_64.rpm                                   487 kB/s | 152 kB     00:00    
(4/9): python3-dateutil-2.8.1-7.el9.noarch.rpm                              799 kB/s | 288 kB     00:00    
(5/9): python3-six-1.15.0-9.el9.noarch.rpm                                  726 kB/s |  37 kB     00:00    
(6/9): python3-dnf-plugins-core-4.3.0-21.el9.noarch.rpm                     2.7 MB/s | 264 kB     00:00    
(7/9): python3-systemd-234-19.el9.x86_64.rpm                                1.7 MB/s |  89 kB     00:00    
(8/9): epel-next-release-9-7.el9.noarch.rpm                                  60 kB/s | 8.1 kB     00:00    
(9/9): epel-release-9-7.el9.noarch.rpm                                      141 kB/s |  19 kB     00:00    
------------------------------------------------------------------------------------------------------------
Total                                                                       1.0 MB/s | 1.0 MB     00:00     
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                                                    1/1 
  Installing       : python3-systemd-234-19.el9.x86_64                                                  1/9 
  Installing       : python3-six-1.15.0-9.el9.noarch                                                    2/9 
  Installing       : python3-dateutil-1:2.8.1-7.el9.noarch                                              3/9 
  Installing       : dbus-libs-1:1.12.20-8.el9.x86_64                                                   4/9 
  Installing       : python3-dbus-1.2.18-2.el9.x86_64                                                   5/9 
  Installing       : python3-dnf-plugins-core-4.3.0-21.el9.noarch                                       6/9 
  Installing       : dnf-plugins-core-4.3.0-21.el9.noarch                                               7/9 
  Installing       : epel-next-release-9-7.el9.noarch                                                   8/9 
  Installing       : epel-release-9-7.el9.noarch                                                        9/9 
  Running scriptlet: epel-release-9-7.el9.noarch                                                        9/9 
Many EPEL packages require the CodeReady Builder (CRB) repository.
It is recommended that you run /usr/bin/crb enable to enable the CRB repository.

  Verifying        : dbus-libs-1:1.12.20-8.el9.x86_64                                                   1/9 
  Verifying        : dnf-plugins-core-4.3.0-21.el9.noarch                                               2/9 
  Verifying        : python3-dateutil-1:2.8.1-7.el9.noarch                                              3/9 
  Verifying        : python3-dbus-1.2.18-2.el9.x86_64                                                   4/9 
  Verifying        : python3-dnf-plugins-core-4.3.0-21.el9.noarch                                       5/9 
  Verifying        : python3-six-1.15.0-9.el9.noarch                                                    6/9 
  Verifying        : python3-systemd-234-19.el9.x86_64                                                  7/9 
  Verifying        : epel-next-release-9-7.el9.noarch                                                   8/9 
  Verifying        : epel-release-9-7.el9.noarch                                                        9/9 

Installed:
  dbus-libs-1:1.12.20-8.el9.x86_64                         dnf-plugins-core-4.3.0-21.el9.noarch            
  epel-next-release-9-7.el9.noarch                         epel-release-9-7.el9.noarch                     
  python3-dateutil-1:2.8.1-7.el9.noarch                    python3-dbus-1.2.18-2.el9.x86_64                
  python3-dnf-plugins-core-4.3.0-21.el9.noarch             python3-six-1.15.0-9.el9.noarch                 
  python3-systemd-234-19.el9.x86_64                       

Complete!
[tony@stapp01 ~]$ exit
logout
Connection to 172.16.238.10 closed.
thor@jumphost ~$ ssh steve@172.16.238.11
The authenticity of host '172.16.238.11 (172.16.238.11)' can't be established.
ED25519 key fingerprint is SHA256:LPufV+31p76m0DEJ4Pn7DeRWMwItaUwMjTViedV43dk.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '172.16.238.11' (ED25519) to the list of known hosts.
steve@172.16.238.11's password: 
Permission denied, please try again.
steve@172.16.238.11's password: 
Last failed login: Wed Jul 16 13:50:32 UTC 2025 from 172.16.238.2 on ssh:notty
There was 1 failed login attempt since the last successful login.
[steve@stapp02 ~]$ sudo dnf install epel-release -y

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for steve: 
CentOS Stream 9 - BaseOS                                                     37 kB/s | 6.7 kB     00:00    
CentOS Stream 9 - BaseOS                                                    9.4 MB/s | 8.8 MB     00:00    
CentOS Stream 9 - AppStream                                                  49 kB/s | 6.8 kB     00:00    
CentOS Stream 9 - AppStream                                                  11 MB/s |  24 MB     00:02    
CentOS Stream 9 - Extras packages                                            24 kB/s | 7.3 kB     00:00    
CentOS Stream 9 - Extras packages                                            68 kB/s |  19 kB     00:00    
Last metadata expiration check: 0:00:01 ago on Wed Jul 16 13:51:01 2025.
Dependencies resolved.
============================================================================================================
 Package                           Architecture    Version                     Repository              Size
============================================================================================================
Installing:
 epel-release                      noarch          9-7.el9                     extras-common           19 k
Installing dependencies:
 dbus-libs                         x86_64          1:1.12.20-8.el9             baseos                 152 k
 python3-dateutil                  noarch          1:2.8.1-7.el9               baseos                 288 k
 python3-dbus                      x86_64          1.2.18-2.el9                baseos                 144 k
 python3-dnf-plugins-core          noarch          4.3.0-21.el9                baseos                 264 k
 python3-six                       noarch          1.15.0-9.el9                baseos                  37 k
 python3-systemd                   x86_64          234-19.el9                  baseos                  89 k
Installing weak dependencies:
 dnf-plugins-core                  noarch          4.3.0-21.el9                baseos                  37 k
 epel-next-release                 noarch          9-7.el9                     extras-common          8.1 k

Transaction Summary
============================================================================================================
Install  9 Packages

Total download size: 1.0 M
Installed size: 3.0 M
Downloading Packages:
(1/9): dnf-plugins-core-4.3.0-21.el9.noarch.rpm                             182 kB/s |  37 kB     00:00    
(2/9): dbus-libs-1.12.20-8.el9.x86_64.rpm                                   589 kB/s | 152 kB     00:00    
(3/9): python3-dateutil-2.8.1-7.el9.noarch.rpm                              991 kB/s | 288 kB     00:00    
(4/9): python3-dbus-1.2.18-2.el9.x86_64.rpm                                 1.4 MB/s | 144 kB     00:00    
(5/9): python3-six-1.15.0-9.el9.noarch.rpm                                  1.0 MB/s |  37 kB     00:00    
(6/9): python3-dnf-plugins-core-4.3.0-21.el9.noarch.rpm                     2.9 MB/s | 264 kB     00:00    
(7/9): python3-systemd-234-19.el9.x86_64.rpm                                1.8 MB/s |  89 kB     00:00    
(8/9): epel-next-release-9-7.el9.noarch.rpm                                 169 kB/s | 8.1 kB     00:00    
(9/9): epel-release-9-7.el9.noarch.rpm                                      280 kB/s |  19 kB     00:00    
------------------------------------------------------------------------------------------------------------
Total                                                                       1.2 MB/s | 1.0 MB     00:00     
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                                                    1/1 
  Installing       : python3-systemd-234-19.el9.x86_64                                                  1/9 
  Installing       : python3-six-1.15.0-9.el9.noarch                                                    2/9 
  Installing       : python3-dateutil-1:2.8.1-7.el9.noarch                                              3/9 
  Installing       : dbus-libs-1:1.12.20-8.el9.x86_64                                                   4/9 
  Installing       : python3-dbus-1.2.18-2.el9.x86_64                                                   5/9 
  Installing       : python3-dnf-plugins-core-4.3.0-21.el9.noarch                                       6/9 
  Installing       : dnf-plugins-core-4.3.0-21.el9.noarch                                               7/9 
  Installing       : epel-next-release-9-7.el9.noarch                                                   8/9 
  Installing       : epel-release-9-7.el9.noarch                                                        9/9 
  Running scriptlet: epel-release-9-7.el9.noarch                                                        9/9 
Many EPEL packages require the CodeReady Builder (CRB) repository.
It is recommended that you run /usr/bin/crb enable to enable the CRB repository.

  Verifying        : dbus-libs-1:1.12.20-8.el9.x86_64                                                   1/9 
  Verifying        : dnf-plugins-core-4.3.0-21.el9.noarch                                               2/9 
  Verifying        : python3-dateutil-1:2.8.1-7.el9.noarch                                              3/9 
  Verifying        : python3-dbus-1.2.18-2.el9.x86_64                                                   4/9 
  Verifying        : python3-dnf-plugins-core-4.3.0-21.el9.noarch                                       5/9 
  Verifying        : python3-six-1.15.0-9.el9.noarch                                                    6/9 
  Verifying        : python3-systemd-234-19.el9.x86_64                                                  7/9 
  Verifying        : epel-next-release-9-7.el9.noarch                                                   8/9 
  Verifying        : epel-release-9-7.el9.noarch                                                        9/9 

Installed:
  dbus-libs-1:1.12.20-8.el9.x86_64                         dnf-plugins-core-4.3.0-21.el9.noarch            
  epel-next-release-9-7.el9.noarch                         epel-release-9-7.el9.noarch                     
  python3-dateutil-1:2.8.1-7.el9.noarch                    python3-dbus-1.2.18-2.el9.x86_64                
  python3-dnf-plugins-core-4.3.0-21.el9.noarch             python3-six-1.15.0-9.el9.noarch                 
  python3-systemd-234-19.el9.x86_64                       

Complete!
[steve@stapp02 ~]$ exit
logout
Connection to 172.16.238.11 closed.
thor@jumphost ~$ ssh banner@172.16.238.12
The authenticity of host '172.16.238.12 (172.16.238.12)' can't be established.
ED25519 key fingerprint is SHA256:+SVWBf2zdWHKVnfaPldAzZO1rHCPu/sYlXBRtiXSbDs.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '172.16.238.12' (ED25519) to the list of known hosts.
banner@172.16.238.12's password: 
Permission denied, please try again.
banner@172.16.238.12's password: 
Last failed login: Wed Jul 16 13:51:38 UTC 2025 from 172.16.238.2 on ssh:notty
There was 1 failed login attempt since the last successful login.
[banner@stapp03 ~]$ sudo dnf install epel-release -y

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for banner: 
CentOS Stream 9 - BaseOS                                                     34 kB/s | 6.7 kB     00:00    
CentOS Stream 9 - BaseOS                                                     12 MB/s | 8.8 MB     00:00    
CentOS Stream 9 - AppStream                                                  31 kB/s | 6.8 kB     00:00    
CentOS Stream 9 - AppStream                                                  27 MB/s |  24 MB     00:00    
CentOS Stream 9 - Extras packages                                            24 kB/s | 7.3 kB     00:00    
CentOS Stream 9 - Extras packages                                            38 kB/s |  19 kB     00:00    
Dependencies resolved.
============================================================================================================
 Package                           Architecture    Version                     Repository              Size
============================================================================================================
Installing:
 epel-release                      noarch          9-7.el9                     extras-common           19 k
Installing dependencies:
 dbus-libs                         x86_64          1:1.12.20-8.el9             baseos                 152 k
 python3-dateutil                  noarch          1:2.8.1-7.el9               baseos                 288 k
 python3-dbus                      x86_64          1.2.18-2.el9                baseos                 144 k
 python3-dnf-plugins-core          noarch          4.3.0-21.el9                baseos                 264 k
 python3-six                       noarch          1.15.0-9.el9                baseos                  37 k
 python3-systemd                   x86_64          234-19.el9                  baseos                  89 k
Installing weak dependencies:
 dnf-plugins-core                  noarch          4.3.0-21.el9                baseos                  37 k
 epel-next-release                 noarch          9-7.el9                     extras-common          8.1 k

Transaction Summary
============================================================================================================
Install  9 Packages

Total download size: 1.0 M
Installed size: 3.0 M
Downloading Packages:
(1/9): dnf-plugins-core-4.3.0-21.el9.noarch.rpm                             260 kB/s |  37 kB     00:00    
(2/9): dbus-libs-1.12.20-8.el9.x86_64.rpm                                   907 kB/s | 152 kB     00:00    
(3/9): python3-dateutil-2.8.1-7.el9.noarch.rpm                              1.3 MB/s | 288 kB     00:00    
(4/9): python3-dnf-plugins-core-4.3.0-21.el9.noarch.rpm                     5.5 MB/s | 264 kB     00:00    
(5/9): python3-dbus-1.2.18-2.el9.x86_64.rpm                                 1.7 MB/s | 144 kB     00:00    
(6/9): python3-six-1.15.0-9.el9.noarch.rpm                                  1.4 MB/s |  37 kB     00:00    
(7/9): python3-systemd-234-19.el9.x86_64.rpm                                3.3 MB/s |  89 kB     00:00    
(8/9): epel-next-release-9-7.el9.noarch.rpm                                  63 kB/s | 8.1 kB     00:00    
(9/9): epel-release-9-7.el9.noarch.rpm                                      125 kB/s |  19 kB     00:00    
------------------------------------------------------------------------------------------------------------
Total                                                                       1.4 MB/s | 1.0 MB     00:00     
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                                                    1/1 
  Installing       : python3-systemd-234-19.el9.x86_64                                                  1/9 
  Installing       : python3-six-1.15.0-9.el9.noarch                                                    2/9 
  Installing       : python3-dateutil-1:2.8.1-7.el9.noarch                                              3/9 
  Installing       : dbus-libs-1:1.12.20-8.el9.x86_64                                                   4/9 
  Installing       : python3-dbus-1.2.18-2.el9.x86_64                                                   5/9 
  Installing       : python3-dnf-plugins-core-4.3.0-21.el9.noarch                                       6/9 
  Installing       : dnf-plugins-core-4.3.0-21.el9.noarch                                               7/9 
  Installing       : epel-next-release-9-7.el9.noarch                                                   8/9 
  Installing       : epel-release-9-7.el9.noarch                                                        9/9 
  Running scriptlet: epel-release-9-7.el9.noarch                                                        9/9 
Many EPEL packages require the CodeReady Builder (CRB) repository.
It is recommended that you run /usr/bin/crb enable to enable the CRB repository.

  Verifying        : dbus-libs-1:1.12.20-8.el9.x86_64                                                   1/9 
  Verifying        : dnf-plugins-core-4.3.0-21.el9.noarch                                               2/9 
  Verifying        : python3-dateutil-1:2.8.1-7.el9.noarch                                              3/9 
  Verifying        : python3-dbus-1.2.18-2.el9.x86_64                                                   4/9 
  Verifying        : python3-dnf-plugins-core-4.3.0-21.el9.noarch                                       5/9 
  Verifying        : python3-six-1.15.0-9.el9.noarch                                                    6/9 
  Verifying        : python3-systemd-234-19.el9.x86_64                                                  7/9 
  Verifying        : epel-next-release-9-7.el9.noarch                                                   8/9 
  Verifying        : epel-release-9-7.el9.noarch                                                        9/9 

Installed:
  dbus-libs-1:1.12.20-8.el9.x86_64                         dnf-plugins-core-4.3.0-21.el9.noarch            
  epel-next-release-9-7.el9.noarch                         epel-release-9-7.el9.noarch                     
  python3-dateutil-1:2.8.1-7.el9.noarch                    python3-dbus-1.2.18-2.el9.x86_64                
  python3-dnf-plugins-core-4.3.0-21.el9.noarch             python3-six-1.15.0-9.el9.noarch                 
  python3-systemd-234-19.el9.x86_64                       

Complete!
[banner@stapp03 ~]$ 
```
