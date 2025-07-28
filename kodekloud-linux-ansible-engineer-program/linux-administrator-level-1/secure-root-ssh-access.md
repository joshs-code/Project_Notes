# Secure Root SSH Access

Following security audits, the `xFusionCorp Industries` security team has rolled out new protocols, including the restriction of direct root SSH login.

Your task is to disable direct SSH root login on all app servers within the `Stratos Datacenter`.



My Solution:

```

[tony@stapp01 ~]$ sudo vi /etc/ssh/sshd_config
[tony@stapp01 ~]$ sudo systemctl restart sshd

[steve@stapp02 ~]$ sudo vi /etc/ssh/sshd_config
[steve@stapp02 ~]$ sudo systemctl restart sshd

[banner@stapp03 ~]$ sudo vi /etc/ssh/sshd_config
[banner@stapp03 ~]$ sudo systemctl restart sshd

==== etc/ssh/sshd_conf ===
PermitRootLogin no
==== end sshd_conf ====


```
