# Managing ACLs Using Ansible

There are some files that need to be created on all app servers in `Stratos DC`. The Nautilus DevOps team want these files to be owned by user `root` only however, they also want that the app specific user to have a set of permissions on these files. All tasks must be done using Ansible only, so they need to create a playbook. Below you can find more information about the task.

Create a playbook named `playbook.yml` under `/home/thor/ansible` directory on `jump host`, an `inventory` file is already present under `/home/thor/ansible` directory on `Jump Server` itself.\


1. Create an empty file `blog.txt` under `/opt/sysops/` directory on app server 1. Set some `acl` properties for this file. Using `acl` provide `read '(r)'` permissions to `group tony` (i.e `entity` is `tony` and `etype` is `group`).
2. Create an empty file `story.txt` under `/opt/sysops/` directory on app server 2. Set some `acl` properties for this file. Using `acl` provide `read + write '(rw)'` permissions to `user steve` (i.e `entity` is `steve` and `etype` is `user`).
3. Create an empty file `media.txt` under `/opt/sysops/` on app server 3. Set some `acl` properties for this file. Using `acl` provide `read + write '(rw)'` permissions to `group banner` (i.e `entity` is `banner` and `etype` is `group`).\


`Note:` Validation will try to run the playbook using command `ansible-playbook -i inventory playbook.yml` so please make sure the playbook works this way, without passing any extra arguments.



My Solution:

<pre><code>thor@jumphost ~$ cd ansible/
thor@jumphost ~/ansible$ vi playbook.yml

===== playbook.yml ======
- name: ACL Tasks App Server 1
  hosts: stapp01
  become: yes
  tasks:
    - name: Create blog file
      ansible.builtin.file:
        state: touch
        path: /opt/sysops/blog.txt
    
    - name: ACL blog file
      ansible.posix.acl:
        path: /opt/sysops/blog.txt
        entity: tony
        etype: group
        permissions: r
        state: present
  
- name: ACL Tasks App Server 2
  hosts: stapp02
  become: yes
  tasks:
    - name: Create story file
      ansible.builtin.file:
        state: touch
        path: /opt/sysops/story.txt
    
    - name: ACL story file
      ansible.posix.acl:
        path: /opt/sysops/story.txt
        entity: steve
        etype: user
        permissions: rw
        state: present

- name: ACL Tasks App Server 3
  hosts: stapp03
  become: yes
  tasks:
    - name: Create media file
      ansible.builtin.file:
        state: touch
        path: /opt/sysops/media.txt
    
    - name: ACL media file
      ansible.posix.acl:
        path: /opt/sysops/media.txt
        entity: banner
        etype: group
        permissions: rw
        state: present

================
<strong>
</strong>thor@jumphost ~/ansible$ ansible-playbook -i inventory playbook.yml 

PLAY [ACL Tasks App Server 1] ***********************************************************************************

TASK [Gathering Facts] ******************************************************************************************
ok: [stapp01]

TASK [Create blog file] *****************************************************************************************
changed: [stapp01]

TASK [ACL blog file] ********************************************************************************************
changed: [stapp01]

PLAY [ACL Tasks App Server 2] ***********************************************************************************

TASK [Gathering Facts] ******************************************************************************************
ok: [stapp02]

TASK [Create story file] ****************************************************************************************
changed: [stapp02]

TASK [ACL story file] *******************************************************************************************
changed: [stapp02]

PLAY [ACL Tasks App Server 3] ***********************************************************************************

TASK [Gathering Facts] ******************************************************************************************
ok: [stapp03]

TASK [Create media file] ****************************************************************************************
changed: [stapp03]

TASK [ACL media file] *******************************************************************************************
changed: [stapp03]

PLAY RECAP ******************************************************************************************************
stapp01                    : ok=3    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
stapp02                    : ok=3    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
stapp03                    : ok=3    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   

</code></pre>
