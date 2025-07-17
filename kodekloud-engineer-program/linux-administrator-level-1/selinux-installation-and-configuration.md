# SElinux Installation and Configuration

Following a security audit, the xFusionCorp Industries security team has opted to enhance application and server security with SELinux. To initiate testing, the following requirements have been established for `App server 1` in the `Stratos Datacenter:`



1. Install the required `SELinux` packages.
2. Permanently disable SELinux for the time being; it will be re-enabled after necessary configuration changes.
3. No need to reboot the server, as a scheduled maintenance reboot is already planned for tonight.
4. Disregard the current status of SELinux via the command line; the final status after the reboot should be `disabled`.



My Solutions:

```
thor@jumphost ~$ ssh tony@172.16.238.10 
The authenticity of host '172.16.238.10 (172.16.238.10)' can't be established.
ED25519 key fingerprint is SHA256:hxt+PdCKebuaV7nCPnhZGDgEhqIZjm5ALxM2/ya/K48.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '172.16.238.10' (ED25519) to the list of known hosts.
tony@172.16.238.10's password: 

[tony@stapp01 ~]$ sudo dnf install policycoreutils selinux-policy-targeted
Last metadata expiration check: 0:03:16 ago on Wed Jul 16 01:18:19 2025.
Package policycoreutils-3.6-2.1.el9.x86_64 is already installed.
Dependencies resolved.
============================================================================================================
 Package                            Architecture      Version                       Repository         Size
============================================================================================================
Installing:
 selinux-policy-targeted            noarch            38.1.60-1.el9                 baseos            6.9 M
Upgrading:
 policycoreutils                    x86_64            3.6-3.el9                     baseos            239 k
 python3-rpm                        x86_64            4.16.1.3-37.el9               baseos             65 k
 rpm                                x86_64            4.16.1.3-37.el9               baseos            536 k
 rpm-build-libs                     x86_64            4.16.1.3-37.el9               baseos             89 k
 rpm-libs                           x86_64            4.16.1.3-37.el9               baseos            308 k
 rpm-sign-libs                      x86_64            4.16.1.3-37.el9               baseos             21 k
Installing dependencies:
 rpm-plugin-selinux                 x86_64            4.16.1.3-37.el9               baseos             17 k
 selinux-policy                     noarch            38.1.60-1.el9                 baseos             44 k

Transaction Summary
============================================================================================================
Install  3 Packages
Upgrade  6 Packages

Total download size: 8.2 M
Is this ok [y/N]: y
Downloading Packages:
(1/9): rpm-plugin-selinux-4.16.1.3-37.el9.x86_64.rpm                         97 kB/s |  17 kB     00:00    
(2/9): selinux-policy-38.1.60-1.el9.noarch.rpm                              226 kB/s |  44 kB     00:00    
(3/9): python3-rpm-4.16.1.3-37.el9.x86_64.rpm                               698 kB/s |  65 kB     00:00    
(4/9): policycoreutils-3.6-3.el9.x86_64.rpm                                 1.5 MB/s | 239 kB     00:00    
(5/9): rpm-build-libs-4.16.1.3-37.el9.x86_64.rpm                            1.5 MB/s |  89 kB     00:00    
(6/9): rpm-4.16.1.3-37.el9.x86_64.rpm                                       4.0 MB/s | 536 kB     00:00    
(7/9): rpm-sign-libs-4.16.1.3-37.el9.x86_64.rpm                             297 kB/s |  21 kB     00:00    
(8/9): rpm-libs-4.16.1.3-37.el9.x86_64.rpm                                  2.7 MB/s | 308 kB     00:00    
(9/9): selinux-policy-targeted-38.1.60-1.el9.noarch.rpm                      13 MB/s | 6.9 MB     00:00    
------------------------------------------------------------------------------------------------------------
Total                                                                        11 MB/s | 8.2 MB     00:00     
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Running scriptlet: selinux-policy-targeted-38.1.60-1.el9.noarch                                       1/1 
  Preparing        :                                                                                    1/1 
  Upgrading        : rpm-libs-4.16.1.3-37.el9.x86_64                                                   1/15 
  Upgrading        : rpm-4.16.1.3-37.el9.x86_64                                                        2/15 
  Upgrading        : policycoreutils-3.6-3.el9.x86_64                                                  3/15 
  Running scriptlet: policycoreutils-3.6-3.el9.x86_64                                                  3/15 
  Installing       : selinux-policy-38.1.60-1.el9.noarch                                               4/15 
  Running scriptlet: selinux-policy-38.1.60-1.el9.noarch                                               4/15 
  Running scriptlet: selinux-policy-targeted-38.1.60-1.el9.noarch                                      5/15 
  Installing       : selinux-policy-targeted-38.1.60-1.el9.noarch                                      5/15 
  Running scriptlet: selinux-policy-targeted-38.1.60-1.el9.noarch                                      5/15 
  Upgrading        : rpm-build-libs-4.16.1.3-37.el9.x86_64                                             6/15 
  Upgrading        : rpm-sign-libs-4.16.1.3-37.el9.x86_64                                              7/15 
  Upgrading        : python3-rpm-4.16.1.3-37.el9.x86_64                                                8/15 
  Installing       : rpm-plugin-selinux-4.16.1.3-37.el9.x86_64                                         9/15 
  Cleanup          : python3-rpm-4.16.1.3-30.el9.x86_64                                               10/15 
  Cleanup          : rpm-build-libs-4.16.1.3-30.el9.x86_64                                            11/15 
  Cleanup          : rpm-sign-libs-4.16.1.3-30.el9.x86_64                                             12/15 
  Running scriptlet: policycoreutils-3.6-2.1.el9.x86_64                                               13/15 
  Cleanup          : policycoreutils-3.6-2.1.el9.x86_64                                               13/15 
  Cleanup          : rpm-libs-4.16.1.3-30.el9.x86_64                                                  14/15 
  Cleanup          : rpm-4.16.1.3-30.el9.x86_64                                                       15/15 
  Running scriptlet: rpm-4.16.1.3-37.el9.x86_64                                                       15/15 
  Running scriptlet: selinux-policy-targeted-38.1.60-1.el9.noarch                                     15/15 
  Running scriptlet: rpm-4.16.1.3-30.el9.x86_64                                                       15/15 
  Verifying        : rpm-plugin-selinux-4.16.1.3-37.el9.x86_64                                         1/15 
  Verifying        : selinux-policy-38.1.60-1.el9.noarch                                               2/15 
  Verifying        : selinux-policy-targeted-38.1.60-1.el9.noarch                                      3/15 
  Verifying        : policycoreutils-3.6-3.el9.x86_64                                                  4/15 
  Verifying        : policycoreutils-3.6-2.1.el9.x86_64                                                5/15 
  Verifying        : python3-rpm-4.16.1.3-37.el9.x86_64                                                6/15 
  Verifying        : python3-rpm-4.16.1.3-30.el9.x86_64                                                7/15 
  Verifying        : rpm-4.16.1.3-37.el9.x86_64                                                        8/15 
  Verifying        : rpm-4.16.1.3-30.el9.x86_64                                                        9/15 
  Verifying        : rpm-build-libs-4.16.1.3-37.el9.x86_64                                            10/15 
  Verifying        : rpm-build-libs-4.16.1.3-30.el9.x86_64                                            11/15 
  Verifying        : rpm-libs-4.16.1.3-37.el9.x86_64                                                  12/15 
  Verifying        : rpm-libs-4.16.1.3-30.el9.x86_64                                                  13/15 
  Verifying        : rpm-sign-libs-4.16.1.3-37.el9.x86_64                                             14/15 
  Verifying        : rpm-sign-libs-4.16.1.3-30.el9.x86_64                                             15/15 

Upgraded:
  policycoreutils-3.6-3.el9.x86_64                   python3-rpm-4.16.1.3-37.el9.x86_64                     
  rpm-4.16.1.3-37.el9.x86_64                         rpm-build-libs-4.16.1.3-37.el9.x86_64                  
  rpm-libs-4.16.1.3-37.el9.x86_64                    rpm-sign-libs-4.16.1.3-37.el9.x86_64                   
Installed:
  rpm-plugin-selinux-4.16.1.3-37.el9.x86_64                 selinux-policy-38.1.60-1.el9.noarch             
  selinux-policy-targeted-38.1.60-1.el9.noarch             

Complete!
[tony@stapp01 ~]$ sudo vi /etc/selinux/config 
```
