# Ansible Lineinfile Module

The Nautilus DevOps team want to install and set up a simple `httpd` web server on all app servers in `Stratos DC`. They also want to deploy a sample web page using Ansible. Therefore, write the required playbook to complete this task as per details mentioned below.

We already have an `inventory` file under `/home/thor/ansible` directory on `jump host`. Write a playbook `playbook.yml` under `/home/thor/ansible` directory on `jump host` itself. Using the playbook perform below given tasks:

1. Install `httpd` web server on all app servers, and make sure its service is up and running.
2. Create a file `/var/www/html/index.html` with content:

`This is a Nautilus sample file, created using Ansible!`\


3. Using `lineinfile` Ansible module add some more content in `/var/www/html/index.html` file. Below is the content:

`Welcome to Nautilus Group!`

Also make sure this new line is added at the top of the file.\


4. The `/var/www/html/index.html` file's user and group `owner` should be `apache` on all app servers.
5. The `/var/www/html/index.html` file's permissions should be `0644` on all app servers.\


`Note:` Validation will try to run the playbook using command `ansible-playbook -i inventory playbook.yml` so please make sure the playbook works this way without passing any extra arguments.



My Solution:

```
vi playbook.yml

===== playbook.yml ====
---
- name: Tasks
  hosts: all
  become: yes
  tasks:
    - name: Install HTTPD
      ansible.builtin.dnf:
        name: httpd
        state: present

    - name: Enable and Start HTTPD
      ansible.builtin.service:
        name: httpd
        enabled: yes
        state: started

    - name: Create Index.html
      ansible.builtin.copy:
        dest: "/var/www/html/index.html"
        content: "This is a Nautilus sample file, created using Ansible!"
        owner: apache
        group: apache
        mode: '0644'

    - name: Add to Index.html
      ansible.builtin.lineinfile:
        path: /var/www/html/index.html
        insertbefore: BOF
        line: 'Welcome to Nautilus Group!'
  ==== end playbook ====
  
  

```
