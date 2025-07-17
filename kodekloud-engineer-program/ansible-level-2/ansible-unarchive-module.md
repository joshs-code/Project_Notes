# Ansible Unarchive Module

One of the DevOps team members has created a zip archive on `jump host` in `Stratos DC` that needs to be extracted and copied over to all app servers in `Stratos DC` itself. Because this is a routine task, the `Nautilus` DevOps team has suggested automating it. We can use Ansible since we have been using it for other automation tasks. Below you can find more details about the task:

We have an `inventory` file under `/home/thor/ansible` directory on `jump host`, which should have all the app servers added already.

There is a zip archive `/usr/src/itadmin/xfusion.zip` on `jump host`.

Create a `playbook.yml` under `/home/thor/ansible/` directory on `jump host` itself to perform the below given tasks.\


1. Unzip `/usr/src/itadmin/xfusion.zip` archive in `/opt/itadmin/` location on all app servers.\

2. Make sure the extracted data must has the respective sudo user as their `user` and `group` owner, i.e tony for app server 1, steve for app server 2, banner for app server 3.\

3. The extracted data permissions must be `0777`.

`Note:` Validation will try to run the playbook using command `ansible-playbook -i inventory playbook.yml` so please make sure playbook works this way, without passing any extra arguments.



My Solution:

```
thor@jumphost ~$ cd ansible/
thor@jumphost ~/ansible$ cat inventory 
stapp01 ansible_host=172.16.238.10 ansible_ssh_pass=Ir0nM@n ansible_user=tony
stapp02 ansible_host=172.16.238.11 ansible_ssh_pass=Am3ric@ ansible_user=steve
stapp03 ansible_host=172.16.238.12 ansible_ssh_pass=BigGr33n ansible_user=bannerthor@jumphost ~/ansible$ 
thor@jumphost ~/ansible$ ls
ansible.cfg  inventory
thor@jumphost ~/ansible$ vi playbook.yml

===== playbook.yml ====

---
- name: unzip content
  hosts: all
  become: true
  tasks:
    - name: unzip archive
      ansible.builtin.unarchive:
        src: /usr/src/itadmin/xfusion.zip
        dest: /opt/itadmin/
        owner: '{{ ansible_user }}'
        group: '{{ ansible_user }}'
        mode: '0777'
        
=== end playbook =====

thor@jumphost ~/ansible$ ansible-playbook -i inventory playbook.yml 

PLAY [unzip content] ***************************************************************************************

TASK [Gathering Facts] *************************************************************************************
ok: [stapp01]
ok: [stapp03]
ok: [stapp02]

TASK [unzip archive] ***************************************************************************************
changed: [stapp01]
changed: [stapp03]
changed: [stapp02]

PLAY RECAP *************************************************************************************************
stapp01                    : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
stapp02                    : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
stapp03                    : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   

```
