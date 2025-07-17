# Custom Apache User Setup

In response to heightened security concerns, the `xFusionCorp Industries` security team has opted for custom Apache users for their web applications. Each user is tailored specifically for an application, enhancing security measures. Your task is to create a custom Apache user according to the outlined specifications:

a. Create a user named `kirsty` on `App server 3` within the Stratos Datacenter.

b. Assign a unique UID `1725` and designate the home directory as `/var/www/kirsty`.



My Solution:

```
thor@jumphost ~$ ssh banner@172.16.238.12
The authenticity of host '172.16.238.12 (172.16.238.12)' can't be established.
ED25519 key fingerprint is SHA256:4k9ngtkOHHzaLlzWkC9fIWxzQmfPz0ZAp4ele7n2eb4.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '172.16.238.12' (ED25519) to the list of known hosts.
banner@172.16.238.12's password: 
[banner@stapp03 ~]$ sudo useradd -m -d  /var/www/kirsty -u 1725 kirsty

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for banner: 
[banner@stapp03 ~]$ tail /etc/passwd
games:x:12:100:games:/usr/games:/sbin/nologin
ftp:x:14:50:FTP User:/var/ftp:/sbin/nologin
nobody:x:65534:65534:Kernel Overflow User:/:/sbin/nologin
tss:x:59:59:Account used for TPM access:/:/usr/sbin/nologin
systemd-coredump:x:999:997:systemd Core Dumper:/:/sbin/nologin
dbus:x:81:81:System message bus:/:/sbin/nologin
sshd:x:74:74:Privilege-separated SSH:/usr/share/empty.sshd:/usr/sbin/nologin
ansible:x:1000:1000::/home/ansible:/bin/bash
banner:x:1001:1001::/home/banner:/bin/bash
kirsty:x:1725:1725::/var/www/kirsty:/bin/bash
[banner@stapp03 ~]$ 
```
