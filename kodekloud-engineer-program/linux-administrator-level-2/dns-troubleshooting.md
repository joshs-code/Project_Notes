# DNS Troubleshooting

The system admins team of xFusionCorp Industries has noticed intermittent issues with DNS resolution in several apps . `App Server 2` in `Stratos Datacenter` is having some DNS resolution issues, so we want to add some additional DNS nameservers on this server.

As a temporary fix we have decided to go with Google public DNS (ipv4). Please make appropriate changes on this server.



My Solution:

```
thor@jumphost ~$ ssh steve@172.16.238.11
The authenticity of host '172.16.238.11 (172.16.238.11)' can't be established.
ED25519 key fingerprint is SHA256:yHfHGhqXCBD4DoZFPnUf9hRwq0W6xDOIQFAWdsrsnEI.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '172.16.238.11' (ED25519) to the list of known hosts.
steve@172.16.238.11's password: 
[steve@stapp02 ~]$ sudo vi /etc/resolv.conf 

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for steve: 

=====
search google.com
nameserver 8.8.8.8
====






```
