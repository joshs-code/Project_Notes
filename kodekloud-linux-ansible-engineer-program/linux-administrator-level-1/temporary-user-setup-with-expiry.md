# Temporary User Setup with Expiry

As part of the temporary assignment to the `Nautilus` project, a developer named `kirsty` requires access for a limited duration. To ensure smooth access management, a temporary user account with an expiry date is needed. Here's what you need to do:

Create a user named `kirsty` on `App Server 1` in Stratos Datacenter. Set the expiry date to `2024-03-28`, ensuring the user is created in lowercase as per standard protocol.



My Solution:

```
thor@jumphost ~$ ssh tony@172.16.238.10
The authenticity of host '172.16.238.10 (172.16.238.10)' can't be established.
ED25519 key fingerprint is SHA256:JcvNyEx8RNTqjK+W4KYqp+hi1Jj88dijC8N1HQIB+bM.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '172.16.238.10' (ED25519) to the list of known hosts.
tony@172.16.238.10's password: 
[tony@stapp01 ~]$ adduser kirsty
adduser: Permission denied.
adduser: cannot lock /etc/passwd; try again later.
[tony@stapp01 ~]$ sudo adduser kirsty

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for tony: 
[tony@stapp01 ~]$ chage -E 2024-03-28
Usage: chage [options] LOGIN

Options:
  -d, --lastday LAST_DAY        set date of last password change to LAST_DAY
  -E, --expiredate EXPIRE_DATE  set account expiration date to EXPIRE_DATE
  -h, --help                    display this help message and exit
  -i, --iso8601                 use YYYY-MM-DD when printing dates
  -I, --inactive INACTIVE       set password inactive after expiration
                                to INACTIVE
  -l, --list                    show account aging information
  -m, --mindays MIN_DAYS        set minimum number of days before password
                                change to MIN_DAYS
  -M, --maxdays MAX_DAYS        set maximum number of days before password
                                change to MAX_DAYS
  -R, --root CHROOT_DIR         directory to chroot into
  -W, --warndays WARN_DAYS      set expiration warning days to WARN_DAYS

[tony@stapp01 ~]$ chage -E 2024-03-28 kirsty
chage: Permission denied.
[tony@stapp01 ~]$ sudo chage -E 2024-03-28 kirsty
[tony@stapp01 ~]$ sudo chage -l kirsty
Last password change                                    : Jul 12, 2025
Password expires                                        : never
Password inactive                                       : never
Account expires                                         : Mar 28, 2024
Minimum number of days between password change          : 0
Maximum number of days between password change          : 99999
Number of days of warning before password expires       : 7
```
