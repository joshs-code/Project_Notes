# Configure Default SSH User for Ansible

The Nautilus DevOps team aims to manage all servers within the stack using Ansible, utilizing a common sudo user across all servers. They plan to use this user for various tasks on each server. While this isn't finalized, they're starting with testing. Ansible is already installed on the `jump host` via yum. Here's the requirement:

On the `jump host`, modify the default configuration of Ansible to enable the use of `ammar` as the default SSH user for all hosts. Ensure to make changes within Ansible's default configuration without creating a new one.



My Solution:

```
thor@jumphost ~$ cd /etc/ansible/
thor@jumphost /etc/ansible$ sudo vi ansible.cfg 

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for thor: 
===== ansible.cfg =====
[defaults]
host_key_checking = False
remote_user=ammar

==== ansible.cfg done ===
```
