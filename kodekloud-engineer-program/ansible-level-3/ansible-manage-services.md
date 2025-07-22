# Ansible Manage Services

Developers are looking for dependencies to be installed and run on Nautilus app servers in Stratos DC. They have shared some requirements with the DevOps team. Because we are now managing packages installation and services management using Ansible, some playbooks need to be created and tested. As per details mentioned below please complete the task:\


a. On `jump host` create an Ansible playbook `/home/thor/ansible/playbook.yml` and configure it to install `vsftpd` on all app servers.

b. After installation make sure to start and enable `vsftpd` service on all app servers.

c. The inventory `/home/thor/ansible/inventory` is already there on `jump host`.

d. Make sure user `thor` should be able to run the playbook on `jump host`.

`Note:` Validation will try to run playbook using command `ansible-playbook -i inventory playbook.yml` so please make sure playbook works this way, without passing any extra arguments.



My Solution:

```
thor@jumphost ~$ cd ansible/
thor@jumphost ~/ansible$ vi playbook.yml

=== playbook.yml ===
---
- name: Tasks
  hosts: all
  become: yes
  tasks:
    
    - name: Install VSFTPD
      ansible.builtin.dnf:
        state: present
        name: vsftpd
    
    - name: Enable and Start
      ansible.builtin.service:
        name: vsftpd
        enabled: yes
        state: started
        
====

thor@jumphost ~/ansible$ ansible-playbook -i inventory playbook.yml 

PLAY [Tasks] ****************************************************************************************************

TASK [Gathering Facts] ******************************************************************************************
ok: [stapp02]
ok: [stapp01]
ok: [stapp03]

TASK [Install VSFTPD] *******************************************************************************************
fatal: [stapp02]: FAILED! => {"changed": false, "module_stderr": "Shared connection to 172.16.238.11 closed.\r\n", "module_stdout": "", "msg": "MODULE FAILURE\nSee stdout/stderr for the exact error", "rc": 137}
fatal: [stapp03]: FAILED! => {"changed": false, "module_stderr": "Shared connection to 172.16.238.12 closed.\r\n", "module_stdout": "", "msg": "MODULE FAILURE\nSee stdout/stderr for the exact error", "rc": 137}
changed: [stapp01]

TASK [Enable and Start] *****************************************************************************************
changed: [stapp01]

PLAY RECAP ******************************************************************************************************
stapp01                    : ok=3    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
stapp02                    : ok=1    changed=0    unreachable=0    failed=1    skipped=0    rescued=0    ignored=0   
stapp03                    : ok=1    changed=0    unreachable=0    failed=1    skipped=0    rescued=0    ignored=0   

thor@jumphost ~/ansible$ ansible-playbook -i inventory playbook.yml 

PLAY [Tasks] ****************************************************************************************************

TASK [Gathering Facts] ******************************************************************************************
ok: [stapp01]
ok: [stapp02]
ok: [stapp03]

TASK [Install VSFTPD] *******************************************************************************************
ok: [stapp01]
fatal: [stapp03]: FAILED! => {"changed": false, "module_stderr": "Shared connection to 172.16.238.12 closed.\r\n", "module_stdout": "", "msg": "MODULE FAILURE\nSee stdout/stderr for the exact error", "rc": 137}
changed: [stapp02]

TASK [Enable and Start] *****************************************************************************************
ok: [stapp01]
changed: [stapp02]

PLAY RECAP ******************************************************************************************************
stapp01                    : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
stapp02                    : ok=3    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
stapp03                    : ok=1    changed=0    unreachable=0    failed=1    skipped=0    rescued=0    ignored=0   

thor@jumphost ~/ansible$ ansible-playbook -i inventory playbook.yml 

PLAY [Tasks] ****************************************************************************************************

TASK [Gathering Facts] ******************************************************************************************
ok: [stapp02]
ok: [stapp01]
ok: [stapp03]

TASK [Install VSFTPD] *******************************************************************************************
ok: [stapp02]
ok: [stapp01]
changed: [stapp03]

TASK [Enable and Start] *****************************************************************************************
ok: [stapp01]
ok: [stapp02]
changed: [stapp03]

PLAY RECAP ******************************************************************************************************
stapp01                    : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
stapp02                    : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
stapp03                    : ok=3    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   

```
