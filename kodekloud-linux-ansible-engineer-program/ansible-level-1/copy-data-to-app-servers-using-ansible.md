# Copy Data to App Servers using Ansible





My solution:

```

thor@jumphost ~$ cd ansible/
thor@jumphost ~/ansible$ vi inventory

=== inventory ===

[app_servers]
stapp01.stratos.xfusioncorp.com ansible_host=172.16.238.10 ansible_user=tony
stapp02.stratos.xfusioncorp.com ansible_host=172.16.238.11 ansible_user=steve
stapp03.stratos.xfusioncorp.com ansible_host=172.16.238.12 ansible_user=banner

=== end inventory ===

==== ansible.cfg ===
[defaults]
remote_host=thor

==== playbook.yml ====
- name: Copy File To App Servers
  hosts: app_servers
  become: true
  tasks:
    - name: copy index.html
      ansible.builtin.copy:
        src: /usr/src/itadmin/index.html
        dest: /opt/itadmin
        
===== End playbook.yml ====

thor@jumphost ~/ansible$ ssh-keygen -t rsa -b 4096
Generating public/private rsa key pair.
Enter file in which to save the key (/home/thor/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/thor/.ssh/id_rsa
Your public key has been saved in /home/thor/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:Lp/FAaPhcrk/bBBIjwxT3Q3RrsOx3by6V/65FcFs3us thor@jumphost.stratos.xfusioncorp.com
The key's randomart image is:
+---[RSA 4096]----+
|   ... .o=       |
|  o . . . o   o  |
|   = +. o.     = |
|    +.o+.o.   o o|
|    . =oS=.o   o.|
|     o.o=...o . o|
|      oo..o  + ..|
|       ++o  o o o|
|       .+.o+   Eo|
+----[SHA256]-----+
thor@jumphost ~/ansible$ ssh-copy-id tony@172.16.238.10
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/thor/.ssh/id_rsa.pub"
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
tony@172.16.238.10's password: 

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'tony@172.16.238.10'"
and check to make sure that only the key(s) you wanted were added.

thor@jumphost ~/ansible$ ssh-copy-id steve@172.16.238.11
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/thor/.ssh/id_rsa.pub"
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
steve@172.16.238.11's password: 

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'steve@172.16.238.11'"
and check to make sure that only the key(s) you wanted were added.

thor@jumphost ~/ansible$ ssh-copy-id banner@172.16.238.12
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/thor/.ssh/id_rsa.pub"
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
banner@172.16.238.12's password: 

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'banner@172.16.238.12'"
and check to make sure that only the key(s) you wanted were added.


thor@jumphost ~/ansible$ ansible-playbook -i inventory playbook.yml 

PLAY [Copy File To App Servers] ****************************************************************************

TASK [Gathering Facts] *************************************************************************************
ok: [stapp02.stratos.xfusioncorp.com]
ok: [stapp01.stratos.xfusioncorp.com]
ok: [stapp03.stratos.xfusioncorp.com]

TASK [copy index.html] *************************************************************************************
changed: [stapp03.stratos.xfusioncorp.com]
changed: [stapp02.stratos.xfusioncorp.com]
changed: [stapp01.stratos.xfusioncorp.com]

PLAY RECAP *************************************************************************************************
stapp01.stratos.xfusioncorp.com : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
stapp02.stratos.xfusioncorp.com : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
stapp03.stratos.xfusioncorp.com : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   

```
