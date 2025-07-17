# Ansible Archive Module

The Nautilus DevOps team has some data on each app server in `Stratos DC` that they want to copy to a different location. However, they want to create an archive of the data first, then they want to copy the same to a different location on the respective app server. Additionally, there are some specific requirements for each server. Perform the task using Ansible playbook as per requirements mentioned below:

Create a playbook named `playbook.yml` under `/home/thor/ansible` directory on `jump host`, an `inventory` file is already placed under `/home/thor/ansible/` directory on `Jump Server` itself.\


1. Create an archive `games.tar.gz` (make sure archive format is `tar.gz`) of `/usr/src/data/` directory ( present on each app server ) and copy it to `/opt/data/` directory on all app servers. The user and group `owner` of archive `games.tar.gz` should be `tony` for `App Server 1`, `steve` for `App Server 2` and `banner` for `App Server 3`.

`Note:` Validation will try to run playbook using command `ansible-playbook -i inventory playbook.yml` so please make sure playbook works this way, without passing any extra arguments.



My Solution:

```
thor@jumphost ~$ ls
ansible
thor@jumphost ~$ cd ansible/
thor@jumphost ~/ansible$ vi playbook.yml

==== playbook.yml ====

- name: Create Tar
  hosts: all
  become: true
  tasks:
    - name: compress
      community.general.archive:
        path: /usr/src/data/
        dest: /opt/data/games.tar.gz
        format: gz
        force_archive: true
        owner: '{{ ansible_user }}'
        group: '{{ ansible_user }}'
        
 ==== playbook end ====
   
thor@jumphost ~/ansible$ ansible-playbook -i inventory playbook.yml 

PLAY [Create Tar] ******************************************************************************************

TASK [Gathering Facts] *************************************************************************************
ok: [stapp01]
ok: [stapp03]
ok: [stapp02]

TASK [compress] ********************************************************************************************
changed: [stapp03]
changed: [stapp02]
changed: [stapp01]

PLAY RECAP *************************************************************************************************
stapp01                    : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
stapp02                    : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
stapp03                    : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0        
```
