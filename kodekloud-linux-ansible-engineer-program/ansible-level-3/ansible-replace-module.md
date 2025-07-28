# Page

There is some data on all app servers in `Stratos DC`. The Nautilus development team shared some requirement with the DevOps team to alter some of the data as per recent changes they made. The DevOps team is working to prepare an Ansible playbook to accomplish the same. Below you can find more details about the task.

Write a `playbook.yml` under `/home/thor/ansible` on `jump host`, an `inventory` is already present under `/home/thor/ansible` directory on `Jump host` itself. Perform below given tasks using this playbook:\


1. We have a file `/opt/data/blog.txt` on app server 1. Using Ansible `replace` module replace string `xFusionCorp` to `Nautilus` in that file.
2. We have a file `/opt/data/story.txt` on app server 2. Using Ansible`replace` module replace the string `Nautilus` to `KodeKloud` in that file.
3. We have a file `/opt/data/media.txt` on app server 3. Using Ansible `replace` module replace string `KodeKloud` to `xFusionCorp Industries` in that file.\


`Note:` Validation will try to run the playbook using command `ansible-playbook -i inventory playbook.yml` so please make sure the playbook works this way without passing any extra arguments.



My Solution:

```
thor@jumphost ~$ cd ansible/
thor@jumphost ~/ansible$ vi playbook.yml

==== Playbook.yml ===

---
- name: Task 5
  hosts: all
  become: yes
  tasks:

    - name: Replace App1
      ansible.builtin.replace:
        path: /opt/data/blog.txt
        regexp: 'xFusionCorp'  # Use 'regexp' instead of 'regex'
        replace: 'Nautilus'
      when: ansible_hostname == "stapp01"

    - name: Replace App2
      ansible.builtin.replace:
        path: /opt/data/story.txt
        regexp: 'Nautilus'  # Use 'regexp' instead of 'regex'
        replace: 'KodeKloud'
      when: ansible_hostname == "stapp02"

    - name: Replace App3
      ansible.builtin.replace:
        path: /opt/data/media.txt
        regexp: 'KodeKloud'  # Use 'regexp' instead of 'regex'
        replace: 'xFusionCorp Industries'
      when: ansible_hostname == "stapp03"

======

thor@jumphost ~/ansible$ ansible-playbook -i inventory playbook.yml 

PLAY [Task 5] ***************************************************************************************************

TASK [Gathering Facts] ******************************************************************************************
ok: [stapp01]
ok: [stapp02]
ok: [stapp03]

TASK [Replace App1] *********************************************************************************************
skipping: [stapp02]
skipping: [stapp03]
changed: [stapp01]

TASK [Replace App2] *********************************************************************************************
skipping: [stapp01]
skipping: [stapp03]
changed: [stapp02]

TASK [Replace App3] *********************************************************************************************
skipping: [stapp01]
skipping: [stapp02]
changed: [stapp03]

PLAY RECAP ******************************************************************************************************
stapp01                    : ok=2    changed=1    unreachable=0    failed=0    skipped=2    rescued=0    ignored=0   
stapp02                    : ok=2    changed=1    unreachable=0    failed=0    skipped=2    rescued=0    ignored=0   
stapp03                    : ok=2    changed=1    unreachable=0    failed=0    skipped=2    rescued=0    ignored=0   
```
