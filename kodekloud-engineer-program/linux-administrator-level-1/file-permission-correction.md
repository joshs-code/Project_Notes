# File Permission Correction

After conducting a security audit within the `Stratos DC`, the Nautilus security team discovered misconfigured permissions on critical files. To address this, corrective actions are being taken by the production support team. Specifically, the file named `/etc/resolv.conf` on `Nautilus App 3` server requires adjustments to its Access Control Lists (ACLs) as follows:

1\. The file's user owner and group owner should be set to `root`.\
\
2\. `Others` should possess `read only` permissions on the file.\
\
3\. User `javed` must not have any permissions on the file.\
\
4\. User `rod` should be granted `read only` permission on the file.



My solution:

```
thor@jumphost ~$ ssh banner@172.16.238.12
The authenticity of host '172.16.238.12 (172.16.238.12)' can't be established.
ED25519 key fingerprint is SHA256:r1y+FDafsxPUj7cEbsG47XqrpxJsb9hAX1zRnUV8TZ4.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '172.16.238.12' (ED25519) to the list of known hosts.
banner@172.16.238.12's password: 
[banner@stapp03 ~]$ getfacl /etc/resolv.conf 
getfacl: Removing leading '/' from absolute path names
# file: etc/resolv.conf
# owner: root
# group: root
user::rw-
group::r--
other::r--

[banner@stapp03 ~]$ setacl -m -u javed:--- /etc/resolv.conf 
-bash: setacl: command not found
[banner@stapp03 ~]$ setfacl -m -u javed:--- /etc/resolv.conf 
setfacl: Option -m: Invalid argument near character 1
[banner@stapp03 ~]$ setfacl -m u javed:--- /etc/resolv.conf 
setfacl: Option -m incomplete
[banner@stapp03 ~]$ setfacl -m u:javed:--- /etc/resolv.conf 
setfacl: /etc/resolv.conf: Operation not permitted
[banner@stapp03 ~]$ sudo setfacl -m u:javed:--- /etc/resolv.conf 

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for banner: 
[banner@stapp03 ~]$ setfacl -m u:rod:r-- /etc/resolv.conf 
setfacl: /etc/resolv.conf: Operation not permitted
[banner@stapp03 ~]$ sudo setfacl -m u:rod:r-- /etc/resolv.conf 
[banner@stapp03 ~]$ getfacl /etc/resolv.conf 
getfacl: Removing leading '/' from absolute path names
# file: etc/resolv.conf
# owner: root
# group: root
user::rw-
user:javed:---
user:rod:r--
group::r--
mask::r--
other::r--
```
