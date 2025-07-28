# Script Execution Permissions

In a bid to automate backup processes, the `xFusionCorp Industries` sysadmin team has developed a new bash script named `xfusioncorp.sh`. While the script has been distributed to all necessary servers, it lacks executable permissions on `App Server 3` within the Stratos Datacenter.

Your task is to grant executable permissions to the `/tmp/xfusioncorp.sh` script on `App Server 3`. Additionally, ensure that all users have the capability to execute it.

My Solution:

```
thor@jumphost ~$ ssh banner@172.16.238.12
The authenticity of host '172.16.238.12 (172.16.238.12)' can't be established.
ED25519 key fingerprint is SHA256:/4Rd4blKAnEf1a95pVUxAWturlaNVQcXM2lfvawPJCo.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '172.16.238.12' (ED25519) to the list of known hosts.
banner@172.16.238.12's password: 
[banner@stapp03 ~]$ sudo chmod 755 /tmp/xfusioncorp.sh 

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for banner: 
[banner@stapp03 ~]$ /tmp/xfusioncorp.sh 
Welcome To KodeKloud
```
