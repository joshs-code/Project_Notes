# Firewall Configuration

The `Nautilus` system admins team has rolled out a web UI application for their backup utility on the `Nautilus backup server` within the `Stratos Datacenter`. This application operates on port `8083`, and `firewalld` is active on the server. To meet operational needs, the following requirements have been identified:

Allow all incoming connections on port `8083/tcp`. Ensure the zone is set to `public`



My Solution:

<pre><code><strong>thor@jumphost ~$ ssh clint@172.16.238.16
</strong>The authenticity of host '172.16.238.16 (172.16.238.16)' can't be established.
ED25519 key fingerprint is SHA256:ovO8nF2na/lme+olipj/EkAxlPIDeEZFUc/d1/73Xrc.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '172.16.238.16' (ED25519) to the list of known hosts.
clint@172.16.238.16's password: 
Permission denied, please try again.
clint@172.16.238.16's password: 
[clint@stbkp01 ~]$ sudo firewall-cmd --add-port=8083/tcp --zone=public --permanent

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for clint: 
success
[clint@stbkp01 ~]$ firewall-cmd --reload
ERROR:dbus.proxies:Introspect error on :1.2:/org/fedoraproject/FirewallD1: dbus.exceptions.DBusException: org.fedoraproject.slip.dbus.service.PolKit.NotAuthorizedException.org.fedoraproject.FirewallD1.info: 
Authorization failed.
    Make sure polkit agent is running or run the application as superuser.
[clint@stbkp01 ~]$ sudo firewall-cmd --reload
success
[clint@stbkp01 ~]$ sudo firewall-cmd --list-all
public
  target: default
  icmp-block-inversion: no
  interfaces: 
  sources: 
  services: dhcpv6-client ssh
  ports: 8083/tcp
  protocols: 
  masquerade: no
  forward-ports: 
  source-ports: 
  icmp-blocks: 
  rich rules: 
</code></pre>
