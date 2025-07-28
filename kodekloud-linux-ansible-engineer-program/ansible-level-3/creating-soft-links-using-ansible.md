# Creating Soft Links Using Ansible

The Nautilus DevOps team is practicing some of the Ansible modules and creating and testing different Ansible playbooks to accomplish tasks. Recently they started testing an Ansible file module to create soft links on all app servers. Below you can find more details about it.\


Write a `playbook.yml` under `/home/thor/ansible` directory on `jump host`, an `inventory` file is already present under `/home/thor/ansible` directory on `jump host` itself. Using this playbook accomplish below given tasks:\


1. Create an empty file `/opt/data/blog.txt` on app server 1; its user owner and group owner should be `tony`. Create a `symbolic link` of source path `/opt/data` to destination `/var/www/html`.
2. Create an empty file `/opt/data/story.txt` on app server 2; its user owner and group owner should be `steve`. Create a `symbolic link` of source path `/opt/data` to destination `/var/www/html`.
3. Create an empty file `/opt/data/media.txt` on app server 3; its user owner and group owner should be `banner`. Create a `symbolic link` of source path `/opt/data` to destination `/var/www/html`.\


`Note`: Validation will try to run the playbook using command `ansible-playbook -i inventory playbook.yml` so please make sure playbook works this way without passing any extra arguments.



My Solution:

```
---
- name: Task for stapp01
  hosts: stapp01
  become: yes
  tasks:

    - name: Create Empty File
      ansible.builtin.file:
        path: /opt/data/blog.txt
        state: touch
        owner: '{{ ansible_user }}'
        group: '{{ ansible_user }}'

    - name: Symlink
      ansible.builtin.file:
        src: /opt/data
        dest: /var/www/html
        state: link
      
- name: Task for stapp02
  hosts: stapp02
  become: yes
  tasks:

    - name: Create Empty File
      ansible.builtin.file:
        path: /opt/data/story.txt
        state: touch
        owner: '{{ ansible_user }}'
        group: '{{ ansible_user }}'

    - name: Symlink
      ansible.builtin.file:
        src: /opt/data
        dest: /var/www/html
        state: link
      
- name: Task for stapp03
  hosts: stapp03
  become: yes
  tasks:

    - name: Create Empty File
      ansible.builtin.file:
        path: /opt/data/media.txt
        state: touch
        owner: '{{ ansible_user }}'
        group: '{{ ansible_user }}'

    - name: Symlink
      ansible.builtin.file:
        src: /opt/data
        dest: /var/www/html
        state: link
      
```

Or you can use a loop for the playbook like this:

```
---
- name: Configure files and symlinks on app servers
  hosts: stapp01,stapp02,stapp03
  become: yes
  tasks:
    - name: Ensure /opt/data directory exists
      ansible.builtin.file:
        path: /opt/data
        state: directory
        owner: '{{ ansible_user }}'
        group: '{{ ansible_user }}'
        mode: '0755'

    - name: Create empty file
      ansible.builtin.file:
        path: "{{ item.path }}"
        state: touch
        owner: '{{ ansible_user }}'
        group: '{{ ansible_user }}'
        mode: '0644'
      loop:
        - { path: '/opt/data/blog.txt', host: 'stapp01' }
        - { path: '/opt/data/story.txt', host: 'stapp02' }
        - { path: '/opt/data/media.txt', host: 'stapp03' }
      when: inventory_hostname == item.host

    - name: Create symlink to /opt/data
      ansible.builtin.file:
        src: /opt/data
        dest: /var/www/html
        state: link
        force: yes
```
