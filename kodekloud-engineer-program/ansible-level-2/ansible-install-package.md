# Ansible Install Package

The Nautilus Application development team wanted to test some applications on app servers in `Stratos Datacenter`. They shared some pre-requisites with the DevOps team, and packages need to be installed on app servers. Since we are already using Ansible for automating such tasks, please perform this task using Ansible as per details mentioned below:

1. Create an inventory file `/home/thor/playbook/inventory` on `jump host` and add all app servers in it.
2. Create an Ansible playbook `/home/thor/playbook/playbook.yml` to install `vim-enhanced` package on `all app servers` using Ansible `yum` module.
3. Make sure user `thor` should be able to run the playbook on `jump host`.

`Note:` Validation will try to run playbook using command `ansible-playbook -i inventory playbook.yml` so please make sure playbook works this way, without passing any extra arguments.



My Solution:

```
thor@jumphost ~$ cd playbook/
thor@jumphost ~/playbook$ ls
thor@jumphost ~/playbook$ vi inventory

==== inventory ====
[app_servers]
stapp01.stratos.xfusioncorp.com ansible_host=172.16.238.10 ansible_user=tony
stapp02.stratos.xfusioncorp.com ansible_host=172.16.238.11 ansible_user=steve
stapp03.stratos.xfusioncorp.com ansible_host=172.16.238.12 ansible_user=banner
======= end inventory ====

thor@jumphost ~/playbook$ vi playbook.yml

=== playbook.yml ====

---
- name: vim package
  hosts: app_servers
  become: true
  tasks:
    - name: install package
      ansible.builtin.yum:
        name: vim-enhanced
        state: present
      
 ==== end playbook ====     
 
 thor@jumphost ~/playbook$ ssh-keygen -t rsa -b 4098
Generating public/private rsa key pair.
Enter file in which to save the key (/home/thor/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/thor/.ssh/id_rsa
Your public key has been saved in /home/thor/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:aRoDUpZH4Ln4miGf3fHCZCjoGWpK0hc5pAYfTtGQ8Pg thor@jumphost.stratos.xfusioncorp.com
The key's randomart image is:
+---[RSA 4098]----+
|..o+++.          |
| o.=o..          |
|o = =.           |
| * * +   .       |
| .E =.o S        |
|.+...oo=         |
|=.=.o+o          |
|+* B .oo         |
|+ = . ...        |
+----[SHA256]-----+
thor@jumphost ~/playbook$ ssh-copy-id tony@172.16.238.10
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/thor/.ssh/id_rsa.pub"
The authenticity of host '172.16.238.10 (172.16.238.10)' can't be established.
ED25519 key fingerprint is SHA256:59Q49BBrXadCZdCcjH8K2JyiSZ1cRkLXI3gHz5GYgVM.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? Ir0nM@n
Please type 'yes', 'no' or the fingerprint: yes
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
tony@172.16.238.10's password: 

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'tony@172.16.238.10'"
and check to make sure that only the key(s) you wanted were added.

thor@jumphost ~/playbook$ ssh-copy-id steve@172.16.238.11
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/thor/.ssh/id_rsa.pub"
The authenticity of host '172.16.238.11 (172.16.238.11)' can't be established.
ED25519 key fingerprint is SHA256:HTZsLExO7uA0N2liw7XX18pCSsXBn0pbEfUBIA7NMhU.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
steve@172.16.238.11's password: 

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'steve@172.16.238.11'"
and check to make sure that only the key(s) you wanted were added.

thor@jumphost ~/playbook$ ssh-copy-id banner@172.16.238.12
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/thor/.ssh/id_rsa.pub"
The authenticity of host '172.16.238.12 (172.16.238.12)' can't be established.
ED25519 key fingerprint is SHA256:2Fp1UQYlusVKAZ/wCKTjEKRzZxdc16DdP8YgSqt6AuI.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
banner@172.16.238.12's password: 
Permission denied, please try again.
banner@172.16.238.12's password: 
Permission denied, please try again.
banner@172.16.238.12's password: 

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'banner@172.16.238.12'"
and check to make sure that only the key(s) you wanted were added.
 
==== ansible.cfg ====

 [defaults]
private_key_file=~/.ssh/id_rsa
host_key_checking=False

===== end ansible.cfg ===

thor@jumphost ~/playbook$ ansible-playbook -i inventory playbook.yml 

PLAY [vim package] *****************************************************************************************

TASK [Gathering Facts] *************************************************************************************
ok: [stapp02.stratos.xfusioncorp.com]
ok: [stapp03.stratos.xfusioncorp.com]
ok: [stapp01.stratos.xfusioncorp.com]

TASK [install package] *************************************************************************************
changed: [stapp03.stratos.xfusioncorp.com]
fatal: [stapp01.stratos.xfusioncorp.com]: FAILED! => {"changed": false, "module_stderr": "Shared connection to 172.16.238.10 closed.\r\n", "module_stdout": "", "msg": "MODULE FAILURE\nSee stdout/stderr for the exact error", "rc": 137}
changed: [stapp02.stratos.xfusioncorp.com]

PLAY RECAP *************************************************************************************************
stapp01.stratos.xfusioncorp.com : ok=1    changed=0    unreachable=0    failed=1    skipped=0    rescued=0    ignored=0   
stapp02.stratos.xfusioncorp.com : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
stapp03.stratos.xfusioncorp.com : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   

thor@jumphost ~/playbook$ vi ansible.cfg


# Adding ansible_python_interpreter=/usr/bin/python3 to my ansible.cfg file since i got error for stapp01
==== ansible.cfg ===

 [defaults]
private_key_file=~/.ssh/id_rsa
host_key_checking=False
ansible_python_interpreter=/usr/bin/python3
 ===== end ansible.cfg ===
 
 thor@jumphost ~/playbook$ ansible-playbook -i inventory playbook.yml 

PLAY [vim package] *****************************************************************************************

TASK [Gathering Facts] *************************************************************************************
ok: [stapp02.stratos.xfusioncorp.com]
ok: [stapp03.stratos.xfusioncorp.com]
ok: [stapp01.stratos.xfusioncorp.com]

TASK [install package] *************************************************************************************
ok: [stapp02.stratos.xfusioncorp.com]
ok: [stapp03.stratos.xfusioncorp.com]
changed: [stapp01.stratos.xfusioncorp.com]

PLAY RECAP *************************************************************************************************
stapp01.stratos.xfusioncorp.com : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
stapp02.stratos.xfusioncorp.com : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
stapp03.stratos.xfusioncorp.com : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
```
