# Timezone Alignment

In the daily standup, it was noted that the timezone settings across the `Nautilus Application Servers` in the `Stratos Datacenter` are inconsistent with the local datacenter's timezone, currently set to `America/Belize`.

Synchronize the timezone settings to match the local datacenter's timezone (`America/Belize`).



My Solution:

```
thor@jumphost ~$ ssh tony@172.16.238.10
The authenticity of host '172.16.238.10 (172.16.238.10)' can't be established.
ED25519 key fingerprint is SHA256:uBXI6IotHyNS7ZkzLIlULszy0I59reWeCp21VXdw0a4.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '172.16.238.10' (ED25519) to the list of known hosts.
tony@172.16.238.10's password: 
Permission denied, please try again.
tony@172.16.238.10's password: 
Permission denied, please try again.
tony@172.16.238.10's password: 
tony@172.16.238.10: Permission denied (publickey,gssapi-keyex,gssapi-with-mic,password).
thor@jumphost ~$ ssh tony@172.16.238.10
tony@172.16.238.10's password: 
Last failed login: Tue Jul 15 11:51:52 UTC 2025 from 172.16.238.3 on ssh:notty
There were 2 failed login attempts since the last successful login.
[tony@stapp01 ~]$ timedatectl
               Local time: Tue 2025-07-15 11:52:04 UTC
           Universal time: Tue 2025-07-15 11:52:04 UTC
                 RTC time: n/a
                Time zone: Etc/UTC (UTC, +0000)
System clock synchronized: yes
              NTP service: n/a
          RTC in local TZ: no
[tony@stapp01 ~]$ sudo timedatectl set-timezone America/Belize

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for tony: 
[tony@stapp01 ~]$ timedatectl
               Local time: Tue 2025-07-15 05:53:34 CST
           Universal time: Tue 2025-07-15 11:53:34 UTC
                 RTC time: n/a
                Time zone: America/Belize (CST, -0600)
System clock synchronized: yes
              NTP service: n/a
          RTC in local TZ: no
[tony@stapp01 ~]$ exit
logout
Connection to 172.16.238.10 closed.
thor@jumphost ~$ ssh steve@172.16.238.11
The authenticity of host '172.16.238.11 (172.16.238.11)' can't be established.
ED25519 key fingerprint is SHA256:eC80K5iLIkPp/uMwyWVC5KbAavrRS+b3rJnQ1MLSins.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '172.16.238.11' (ED25519) to the list of known hosts.
steve@172.16.238.11's password: 
[steve@stapp02 ~]$ sudo timedatectl set-timezone America/Belize

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for steve: 
[steve@stapp02 ~]$ timedatectl
               Local time: Tue 2025-07-15 05:54:23 CST
           Universal time: Tue 2025-07-15 11:54:23 UTC
                 RTC time: n/a
                Time zone: America/Belize (CST, -0600)
System clock synchronized: yes
              NTP service: n/a
          RTC in local TZ: no
[steve@stapp02 ~]$ exit
logout
Connection to 172.16.238.11 closed.
thor@jumphost ~$ ssh banner@172.16.238.12
The authenticity of host '172.16.238.12 (172.16.238.12)' can't be established.
ED25519 key fingerprint is SHA256:/aAoMAlLCBgahah/6hd0anZR8yRXCJQ8TH9/PsxAlkw.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '172.16.238.12' (ED25519) to the list of known hosts.
banner@172.16.238.12's password: 
Permission denied, please try again.
banner@172.16.238.12's password: 
Last failed login: Tue Jul 15 11:54:47 UTC 2025 from 172.16.238.3 on ssh:notty
There was 1 failed login attempt since the last successful login.
[banner@stapp03 ~]$ sudo timedatectl set-timezone America/Belize

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for banner: 
[banner@stapp03 ~]$ timedatectl
               Local time: Tue 2025-07-15 05:55:11 CST
           Universal time: Tue 2025-07-15 11:55:11 UTC
                 RTC time: n/a
                Time zone: America/Belize (CST, -0600)
System clock synchronized: yes
              NTP service: n/a
          RTC in local TZ: no
```
