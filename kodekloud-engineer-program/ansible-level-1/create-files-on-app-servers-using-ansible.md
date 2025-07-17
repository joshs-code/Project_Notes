# Create Files on App Servers using Ansible

The Nautilus DevOps team is testing various Ansible modules on servers in `Stratos DC`. They're currently focusing on file creation on remote hosts using Ansible. Here are the details:

a. Create an inventory file `~/playbook/inventory` on `jump host` and include `all app servers`.

b. Create a playbook `~/playbook/playbook.yml` to create a blank file `/tmp/appdata.txt` on `all app servers`.

c. Set the permissions of the `/tmp/appdata.txt` file to `0655`.

d. Ensure the user/group owner of the `/tmp/appdata.txt` file is `tony` on `app server 1`, `steve` on `app server 2` and `banner` on `app server 3`.

`Note:` Validation will execute the playbook using the command `ansible-playbook -i inventory playbook.yml`, so ensure the playbook functions correctly without any additional arguments.



My Solution:

```

vi ~/playbook/inventory

==== inventory ===
[app_servers]
stapp01.stratos.xfusioncorp.com ansible_host=172.16.238.10 ansible_user=tony
stapp02.stratos.xfusioncorp.com ansible_host=172.16.238.11 ansible_user=steve
stapp03.stratos.xfusioncorp.com ansible_host=172.16.238.12 ansible_user=banner
==== end inventory ===

thor@jumphost ~$ ssh-keygen -t rsa -b 4096
Generating public/private rsa key pair.

Enter file in which to save the key (/home/thor/.ssh/id_rsa): Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/thor/.ssh/id_rsa
Your public key has been saved in /home/thor/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:+UpG4zXAddg+MNL5MW8ud72O/jfshN8Aj7mFd0tL0AA thor@jumphost.stratos.xfusioncorp.com
The key's randomart image is:
+---[RSA 4096]----+
|         ..E.    |
|       ...*.=    |
|        o. = =   |
|         o  + =  |
|        S o .= ..|
|       o + ..*= o|
|        + . ++*=o|
|       o .   +**=|
|        .   oo+B+|
+----[SHA256]-----+
thor@jumphost ~$ ssh-copy-id tony@172.16.238.10
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/thor/.ssh/id_rsa.pub"
The authenticity of host '172.16.238.10 (172.16.238.10)' can't be established.
ED25519 key fingerprint is SHA256:3pqLq2iOXzo7fWkhvbHQSzQoA/UlT2a8O5NnNdoxFzI.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
tony@172.16.238.10's password: 

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'tony@172.16.238.10'"
and check to make sure that only the key(s) you wanted were added.

thor@jumphost ~$ ssh-copy-id steve@172.16.238.11
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/thor/.ssh/id_rsa.pub"
The authenticity of host '172.16.238.11 (172.16.238.11)' can't be established.
ED25519 key fingerprint is SHA256:M4/7oqfdnEc9MLc/SHwZoY1ZVfMt5vUqfjrjHBVFxcw.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
steve@172.16.238.11's password: 

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'steve@172.16.238.11'"
and check to make sure that only the key(s) you wanted were added.

thor@jumphost ~$ ssh-copy-id banner@172.16.238.12
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/thor/.ssh/id_rsa.pub"
The authenticity of host '172.16.238.12 (172.16.238.12)' can't be established.
ED25519 key fingerprint is SHA256:2ajjCMBjCm+W9wVuz4sgTob3pQPCwz8FtwNziTo05SY.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
banner@172.16.238.12's password: 

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'banner@172.16.238.12'"
and check to make sure that only the key(s) you wanted were added.

thor@jumphost ~$ vi playbook/playbook.yml

==== playbook.yml ====

---
- name: Blank file Creation
  hosts: app_servers
  become: true
  tasks:
    - name: Create file
      ansible.builtin.file:
        path: /tmp/appdata.txt
        state: touch
        mode: '0655'
        owner:
          "{{ ansible_user }}"
        group:
          "{{ ansible_user }}"
        
===== end playbook.yml ====

thor@jumphost ~/playbook$ ansible-playbook -i inventory playbook.yml 

PLAY [Blank file Creation] *********************************************************************************

TASK [Gathering Facts] *************************************************************************************
ok: [stapp01.stratos.xfusioncorp.com]
ok: [stapp02.stratos.xfusioncorp.com]
ok: [stapp03.stratos.xfusioncorp.com]

TASK [Create file] *****************************************************************************************
changed: [stapp02.stratos.xfusioncorp.com]
changed: [stapp01.stratos.xfusioncorp.com]
changed: [stapp03.stratos.xfusioncorp.com]

PLAY RECAP *************************************************************************************************
stapp01.stratos.xfusioncorp.com : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
stapp02.stratos.xfusioncorp.com : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
stapp03.stratos.xfusioncorp.com : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   

```
