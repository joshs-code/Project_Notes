# Troubleshoot and Create Ansible Playbook

An Ansible playbook needs completion on the `jump host`, where a team member left off. Below are the details:\


1. The inventory file `/home/thor/ansible/inventory` requires adjustments. The playbook must run on `App Server 3` in `Stratos DC`. Update the inventory accordingly.\

2. Create a playbook `/home/thor/ansible/playbook.yml`. Include a task to create an empty file `/tmp/file.txt` on `App Server 3`.

`Note:` Validation will run the playbook using the command `ansible-playbook -i inventory playbook.yml`. Ensure the playbook works without any additional arguments.



My Solution:

```
thor@jumphost ~$ ssh-keygen -t rsa -b 4096
Generating public/private rsa key pair.
Enter file in which to save the key (/home/thor/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/thor/.ssh/id_rsa
Your public key has been saved in /home/thor/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:tY1mi1dRhtWHVVZ0be+yltvJl/pa18vDEjfJ8klkcN0 thor@jumphost.stratos.xfusioncorp.com
The key's randomart image is:
+---[RSA 4096]----+
|             o+=&|
|            .+.oE|
|          . . o.o|
|         . + . o.|
|        S = o +..|
|         + o o.*o|
|        . o   B=*|
|         .   .BB=|
|             +*B+|
+----[SHA256]-----+
thor@jumphost ~$ ssh-copy-id banner@172.16.238.12
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/thor/.ssh/id_rsa.pub"
The authenticity of host '172.16.238.12 (172.16.238.12)' can't be established.
ED25519 key fingerprint is SHA256:wbZw74lwZLFhaRbUDQE+Wh8s57/1Q3RGjjrJc6K72Vc.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
banner@172.16.238.12's password: 

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'banner@172.16.238.12'"
and check to make sure that only the key(s) you wanted were added.


thor@jumphost ~$ vi /home/thor/ansible/inventory 

====  inventory file: Updated the ip, hostname and username ===
stapp03 ansible_host=172.16.238.12 ansible_user=banner ansible_ssh_common_args='-o StrictHostKeyChecking=no'
=== inventory file done =====


thor@jumphost ~$ vi /home/thor/ansible/playbook.yml

==== playbook.yml ====
---
- name: Create empty file
  hosts: stapp03
  tasks:
  - name: Empty file creation
    file:
      path: /tmp/file.txt
      state: touch
==== playbook end ===

thor@jumphost ~/ansible$ ansible -m ping -i inventory stapp03
stapp03 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}

thor@jumphost ~/ansible$ ansible-playbook -i inventory playbook.yml 

PLAY [Create empty file] ***********************************************************************************

TASK [Gathering Facts] *************************************************************************************
ok: [stapp03]

TASK [Empty file creation] *********************************************************************************
changed: [stapp03]

PLAY RECAP *************************************************************************************************
stapp03                    : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
```
