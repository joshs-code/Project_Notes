# Linux User Setup with Non-Interactive Shell

To accommodate the backup agent tool's specifications, the system admin team at `xFusionCorp Industries` requires the creation of a user with a non-interactive shell. Here's your task:

Create a user named `kirsty` with a non-interactive shell on `App Server 3`.



My Solution:

```
thor@jumphost ~$ ssh banner@172.16.238.12
The authenticity of host '172.16.238.12 (172.16.238.12)' can't be established.
ED25519 key fingerprint is SHA256:KLJN/WJqVdW1h8uBqT2feygIMo65/IrfTtq+UiUk3P0.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '172.16.238.12' (ED25519) to the list of known hosts.
banner@172.16.238.12's password: 
[banner@stapp03 ~]$ sudo adduser -s /sbin/nologin kirsty
[banner@stapp03 ~]$ tail -n 1 /etc/passwd
kirsty:x:1002:1002::/home/kirsty:/sbin/nologin
```
