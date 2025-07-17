# Ansible Ping Module

The Nautilus DevOps team is planning to test several Ansible playbooks on different app servers in `Stratos DC`. Before that, some pre-requisites must be met. Essentially, the team needs to set up a password-less SSH connection between Ansible controller and Ansible managed nodes. One of the tickets is assigned to you; please complete the task as per details mentioned below:

a. `Jump host` is our Ansible controller, and we are going to run Ansible playbooks through `thor` user from `jump host`.

b. There is an inventory file `/home/thor/ansible/inventory` on `jump host`. Using that inventory file test `Ansible ping` from `jump host` to `App Server 2`, make sure ping works.



My Solution:&#x20;

```

thor@jumphost ~$ ansible -m ping -i ansible/inventory stapp02
stapp02 | UNREACHABLE! => {
    "changed": false,
    "msg": "Invalid/incorrect password: Warning: Permanently added '172.16.238.11' (ED25519) to the list of known hosts.\r\nPermission denied, please try again.",
    "unreachable": true
}

thor@jumphost ~$ vi ansible/inventory 

# Added ansible_user=steve
==== inventory ====
stapp01 ansible_host=172.16.238.10 ansible_ssh_pass=Ir0nM@n
stapp02 ansible_host=172.16.238.11 ansible_user=steve ansible_ssh_pass=Am3ric@
stapp03 ansible_host=172.16.238.12 ansible_ssh_pass=BigGr33n
==== end inventory ====

thor@jumphost ~$ ansible -m ping -i ansible/inventory stapp02
```
