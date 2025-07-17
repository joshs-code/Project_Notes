# Process Limit Adjustment

In the `Stratos Datacenter`, our `Storage server` is encountering performance degradation due to excessive processes held by the `nfsuser` user. To mitigate this issue, we need to enforce limitations on its maximum processes. Please set the maximum process limits as specified below:

a. Set the soft limit to `1026`

b. Set the hard limit to `2027`&#x20;



My Solution:

```
ssh natasha@172.16.238.15      
The authenticity of host '172.16.238.15 (172.16.238.15)' can't be established.
ED25519 key fingerprint is SHA256:4jb0QOHnYdhroy4+cAYKW+OIIkznJzBkslLrVqZXC+U.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '172.16.238.15' (ED25519) to the list of known hosts.
natasha@172.16.238.15's password: 
Permission denied, please try again.
natasha@172.16.238.15's password: 
Last failed login: Tue Jul 15 12:28:08 UTC 2025 from 172.16.238.3 on ssh:notty
There was 1 failed login attempt since the last successful login.

[natasha@ststor01 ~]$ sudo vi /etc/security/limits.conf 

==== etc/security/limit.conf ===
nfsuser          hard    nproc           2027
nfsuser          soft    nproc           1024
===== end ===

```
