# Configure Local Yum repos

The `Nautilus` production support team and security team had a meeting last month in which they decided to use local yum repositories for maintaing packages needed for their servers. For now they have decided to configure a local yum repo on `Nautilus Backup Server`. This is one of the pending items from last month, so please configure a local yum repository on `Nautilus Backup Server` as per details given below.

a. We have some packages already present at location `/packages/downloaded_rpms/` on `Nautilus Backup Server`.

b. Create a yum repo named `epel_local` and make sure to set `Repository ID` to `epel_local`. Configure it to use package's location `/packages/downloaded_rpms/`.

c. Install package `samba` from this newly created repo.



My Solution:

```
thor@jumphost ~$ ssh clint@172.16.238.16
The authenticity of host '172.16.238.16 (172.16.238.16)' can't be established.
ED25519 key fingerprint is SHA256:obI7NIcuOMwwO21EwmNZEv/M6jDSaDaOse0CKl9CruA.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '172.16.238.16' (ED25519) to the list of known hosts.
clint@172.16.238.16's password: 
[clint@stbkp01 ~]$ sudo vi /etc/yum.repos.d/epel_local

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for clint: 

===== etc/yum.repos.d/epel_local =====
[epel_local]
name=epel_local
baseurl=file:///packages/downloaded_rpms/
repo_gpgcheck=0
enabled=1


[clint@stbkp01 ~]$ sudo dnf update
Error: There are no enabled repositories in "/etc/yum.repos.d", "/etc/yum/repos.d", "/etc/distro.repos.d".
[clint@stbkp01 ~]$ sudo vi /etc/yum.repos.d/epel_local
[clint@stbkp01 ~]$ sudo vi /etc/yum.repos.d/epel_local
[clint@stbkp01 ~]$ sudo dnf update
Error: There are no enabled repositories in "/etc/yum.repos.d", "/etc/yum/repos.d", "/etc/distro.repos.d".
[clint@stbkp01 ~]$ sudo dnf clean all
50 files removed
[clint@stbkp01 ~]$ sudo dnf update
Error: There are no enabled repositories in "/etc/yum.repos.d", "/etc/yum/repos.d", "/etc/distro.repos.d".
[clint@stbkp01 ~]$ sudo dnf repolist
No repositories available
[clint@stbkp01 ~]$ sudo /etc/yum.repos.d/epel_local /etc/yum.repos.d/epel_local.repo
sudo: /etc/yum.repos.d/epel_local: command not found
[clint@stbkp01 ~]$ sudo mv  /etc/yum.repos.d/epel_local /etc/yum.repos.d/epel_local.repo
[clint@stbkp01 ~]$ sudo dnf repolist
repo id                                                  repo name
epel_local                                               epel_local
[clint@stbkp01 ~]$ sudo dnf install samba
epel_local                                                                        19 MB/s |  56 kB     00:00    
Dependencies resolved.
=================================================================================================================
 Package                           Architecture      Version                         Repository             Size
=================================================================================================================
Installing:
 samba                             x86_64            4.20.0-103.el9                  epel_local            1.0 M
Installing dependencies:
 avahi-libs                        x86_64            0.8-20.el9                      epel_local             68 k
 cups-libs                         x86_64            1:2.3.3op2-26.el9               epel_local            262 k
 glibc-gconv-extra                 x86_64            2.34-105.el9                    epel_local            1.7 M
 jansson                           x86_64            2.14-1.el9                      epel_local             46 k
 libicu                            x86_64            67.1-9.el9                      epel_local            9.6 M
 libldb                            x86_64            2.9.0-1.el9                     epel_local            190 k
 libnetapi                         x86_64            4.20.0-103.el9                  epel_local            145 k
 libtalloc                         x86_64            2.4.2-1.el9                     epel_local             31 k
 libtdb                            x86_64            1.4.10-1.el9                    epel_local             51 k
 libtevent                         x86_64            0.16.1-1.el9                    epel_local             48 k
 libtirpc                          x86_64            1.3.3-6.el9                     epel_local             93 k
 libwbclient                       x86_64            4.20.0-103.el9                  epel_local             43 k
 lmdb-libs                         x86_64            0.9.29-3.el9                    epel_local             61 k
 samba-client-libs                 x86_64            4.20.0-103.el9                  epel_local            5.3 M
 samba-common                      noarch            4.20.0-103.el9                  epel_local            150 k
 samba-common-libs                 x86_64            4.20.0-103.el9                  epel_local            101 k
 samba-common-tools                x86_64            4.20.0-103.el9                  epel_local            483 k
 samba-dcerpc                      x86_64            4.20.0-103.el9                  epel_local            719 k
 samba-ldb-ldap-modules            x86_64            4.20.0-103.el9                  epel_local             28 k
 samba-libs                        x86_64            4.20.0-103.el9                  epel_local            128 k

Transaction Summary
=================================================================================================================
Install  21 Packages

Total size: 20 M
Installed size: 72 M
Is this ok [y/N]: y
Downloading Packages:
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                                                         1/1 
  Installing       : libtalloc-2.4.2-1.el9.x86_64                                                           1/21 
  Installing       : libtevent-0.16.1-1.el9.x86_64                                                          2/21 
  Installing       : libtdb-1.4.10-1.el9.x86_64                                                             3/21 
  Running scriptlet: samba-common-4.20.0-103.el9.noarch                                                     4/21 
  Installing       : samba-common-4.20.0-103.el9.noarch                                                     4/21 
  Running scriptlet: samba-common-4.20.0-103.el9.noarch                                                     4/21 
  Installing       : jansson-2.14-1.el9.x86_64                                                              5/21 
  Installing       : avahi-libs-0.8-20.el9.x86_64                                                           6/21 
  Installing       : cups-libs-1:2.3.3op2-26.el9.x86_64                                                     7/21 
  Installing       : lmdb-libs-0.9.29-3.el9.x86_64                                                          8/21 
  Installing       : libldb-2.9.0-1.el9.x86_64                                                              9/21 
  Installing       : libtirpc-1.3.3-6.el9.x86_64                                                           10/21 
  Installing       : libicu-67.1-9.el9.x86_64                                                              11/21 
  Installing       : glibc-gconv-extra-2.34-105.el9.x86_64                                                 12/21 
  Running scriptlet: glibc-gconv-extra-2.34-105.el9.x86_64                                                 12/21 
  Running scriptlet: libwbclient-4.20.0-103.el9.x86_64                                                     13/21 
  Installing       : libwbclient-4.20.0-103.el9.x86_64                                                     13/21 
  Installing       : samba-common-libs-4.20.0-103.el9.x86_64                                               14/21 
  Installing       : samba-client-libs-4.20.0-103.el9.x86_64                                               15/21 
  Installing       : libnetapi-4.20.0-103.el9.x86_64                                                       16/21 
  Installing       : samba-libs-4.20.0-103.el9.x86_64                                                      17/21 
  Installing       : samba-dcerpc-4.20.0-103.el9.x86_64                                                    18/21 
  Installing       : samba-ldb-ldap-modules-4.20.0-103.el9.x86_64                                          19/21 
  Installing       : samba-common-tools-4.20.0-103.el9.x86_64                                              20/21 
  Installing       : samba-4.20.0-103.el9.x86_64                                                           21/21 
  Running scriptlet: samba-4.20.0-103.el9.x86_64                                                           21/21 
  Verifying        : avahi-libs-0.8-20.el9.x86_64                                                           1/21 
  Verifying        : cups-libs-1:2.3.3op2-26.el9.x86_64                                                     2/21 
  Verifying        : glibc-gconv-extra-2.34-105.el9.x86_64                                                  3/21 
  Verifying        : jansson-2.14-1.el9.x86_64                                                              4/21 
  Verifying        : libicu-67.1-9.el9.x86_64                                                               5/21 
  Verifying        : libldb-2.9.0-1.el9.x86_64                                                              6/21 
  Verifying        : libnetapi-4.20.0-103.el9.x86_64                                                        7/21 
  Verifying        : libtalloc-2.4.2-1.el9.x86_64                                                           8/21 
  Verifying        : libtdb-1.4.10-1.el9.x86_64                                                             9/21 
  Verifying        : libtevent-0.16.1-1.el9.x86_64                                                         10/21 
  Verifying        : libtirpc-1.3.3-6.el9.x86_64                                                           11/21 
  Verifying        : libwbclient-4.20.0-103.el9.x86_64                                                     12/21 
  Verifying        : lmdb-libs-0.9.29-3.el9.x86_64                                                         13/21 
  Verifying        : samba-4.20.0-103.el9.x86_64                                                           14/21 
  Verifying        : samba-client-libs-4.20.0-103.el9.x86_64                                               15/21 
  Verifying        : samba-common-4.20.0-103.el9.noarch                                                    16/21 
  Verifying        : samba-common-libs-4.20.0-103.el9.x86_64                                               17/21 
  Verifying        : samba-common-tools-4.20.0-103.el9.x86_64                                              18/21 
  Verifying        : samba-dcerpc-4.20.0-103.el9.x86_64                                                    19/21 
  Verifying        : samba-ldb-ldap-modules-4.20.0-103.el9.x86_64                                          20/21 
  Verifying        : samba-libs-4.20.0-103.el9.x86_64                                                      21/21 

Installed:
  avahi-libs-0.8-20.el9.x86_64                         cups-libs-1:2.3.3op2-26.el9.x86_64                       
  glibc-gconv-extra-2.34-105.el9.x86_64                jansson-2.14-1.el9.x86_64                                
  libicu-67.1-9.el9.x86_64                             libldb-2.9.0-1.el9.x86_64                                
  libnetapi-4.20.0-103.el9.x86_64                      libtalloc-2.4.2-1.el9.x86_64                             
  libtdb-1.4.10-1.el9.x86_64                           libtevent-0.16.1-1.el9.x86_64                            
  libtirpc-1.3.3-6.el9.x86_64                          libwbclient-4.20.0-103.el9.x86_64                        
  lmdb-libs-0.9.29-3.el9.x86_64                        samba-4.20.0-103.el9.x86_64                              
  samba-client-libs-4.20.0-103.el9.x86_64              samba-common-4.20.0-103.el9.noarch                       
  samba-common-libs-4.20.0-103.el9.x86_64              samba-common-tools-4.20.0-103.el9.x86_64                 
  samba-dcerpc-4.20.0-103.el9.x86_64                   samba-ldb-ldap-modules-4.20.0-103.el9.x86_64             
  samba-libs-4.20.0-103.el9.x86_64                    

Complete!
[clint@stbkp01 ~]$ 

```
