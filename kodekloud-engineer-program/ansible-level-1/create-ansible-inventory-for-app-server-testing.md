# Create Ansible Inventory for App Server Testing

The Nautilus DevOps team is testing Ansible playbooks on various servers within their stack. They've placed some playbooks under `/home/thor/playbook/` directory on the `jump host` and now intend to test them on `app server 3` in `Stratos DC`. However, an inventory file needs creation for Ansible to connect to the respective app. Here are the requirements:

a. Create an ini type Ansible inventory file `/home/thor/playbook/inventory` on `jump host`.

b. Include `App Server 3` in this inventory along with necessary variables for proper functionality.

c. Ensure the inventory hostname corresponds to the `server name` as per the wiki, for example `stapp01` for `app server 1` in `Stratos DC`.

`Note:` Validation will execute the playbook using the command `ansible-playbook -i inventory playbook.yml`. Ensure the playbook functions properly without any extra arguments.



My solution:

Mistakes i made:

1. Didnt add spaces in the inventory file for the groups
2. Forgot to add "ansible\_user=banner" in my inventory.

Simple fixes but just noting for future

<pre><code>thor@jumphost ~$ vi /home/thor/playbook/inventory

<strong>==== inventory.ini ======
</strong>[App Server 3] 
stapp03.stratos.xfusioncorp.com ansible_host=172.16.238.12 
===== inventory end =====

thor@jumphost ~/playbook$ ssh-keygen -t rsa -b 4096
Generating public/private rsa key pair.
Enter file in which to save the key (/home/thor/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/thor/.ssh/id_rsa
Your public key has been saved in /home/thor/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:xjqAn7t555AnRBHnmRI8JUO3am3D1MkEigfGHppophk thor@jumphost.stratos.xfusioncorp.com
The key's randomart image is:
+---[RSA 4096]----+
|   .ooBo=..      |
|   .oooX B .     |
| . +..=.* +      |
|Eoo..o B         |
|+o. . + S        |
|o  . = = .       |
|    o * .        |
|     o.=.        |
|    +o o.        |
+----[SHA256]-----+
thor@jumphost ~/playbook$ ssh-copy-id banner@172.16.238.12
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/thor/.ssh/id_rsa.pub"
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
banner@172.16.238.12's password: 

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'banner@172.16.238.12'"
and check to make sure that only the key(s) you wanted were added.

ansible-playbook -i inventory.ini playbook.yml 

PLAY [all] *************************************************************************************************

TASK [Gathering Facts] *************************************************************************************
fatal: [stapp03.stratos.xfusioncorp.com]: UNREACHABLE! => {"changed": false, "msg": "Failed to connect to the host via ssh: thor@172.16.238.12: Permission denied (publickey,gssapi-keyex,gssapi-with-mic,password).", "unreachable": true}

thor@jumphost ~/playbook$ vi inventory

==== inventory.ini ======
[app_server_3] # No Spaces use underscores
stapp03.stratos.xfusioncorp.com ansible_host=172.16.238.12 ansible_user=banner
===== inventory end =====

thor@jumphost ~/playbook$ ansible-playbook -i inventory playbook.yml 

PLAY [all] *************************************************************************************************

TASK [Gathering Facts] *************************************************************************************
ok: [stapp03.stratos.xfusioncorp.com]

TASK [Install httpd package] *******************************************************************************
changed: [stapp03.stratos.xfusioncorp.com]

TASK [Start service httpd] *********************************************************************************
changed: [stapp03.stratos.xfusioncorp.com]

PLAY RECAP *************************************************************************************************
stapp03.stratos.xfusioncorp.com : ok=3    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
</code></pre>
