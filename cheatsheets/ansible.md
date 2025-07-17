# Ansible

## Ansible Cheat Sheet

This cheat sheet provides a quick reference for Ansible commands, configuration files, and their purposes.&#x20;

### Table of Contents

1. Common Ansible Commands
2. Sample Ansible Files
   * Inventory File
   * ansible.cfg
   * General Playbook
   * Role Structure
3. Key Concepts

***

### Common Ansible Commands

| **Command**                                      | **Description**                                                                                                        |
| ------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------- |
| `ansible --version`                              | Displays the installed Ansible version and configuration details.                                                      |
| `ansible-inventory --list`                       | Lists all hosts and groups in the inventory file in JSON format.                                                       |
| `ansible-inventory --graph`                      | Displays a visual representation of the inventory structure.                                                           |
| `ansible all -m ping`                            | Tests connectivity to all hosts in the inventory using the `ping` module. Replace `all` with a specific group or host. |
| `ansible <host/group> -m command -a "command"`   | Runs an ad-hoc command on specified hosts (e.g., `ansible webservers -m command -a "uptime"`).                         |
| `ansible-playbook playbook.yml`                  | Executes the specified playbook.                                                                                       |
| `ansible-playbook playbook.yml --check`          | Runs the playbook in dry-run mode to simulate changes without applying them.                                           |
| `ansible-playbook playbook.yml --diff`           | Shows differences in files or templates before and after applying changes (useful for configuration management).       |
| `ansible-playbook playbook.yml -i inventory.yml` | Runs a playbook using a specific inventory file.                                                                       |
| `ansible-vault create secret.yml`                | Creates an encrypted file for storing sensitive data (e.g., passwords).                                                |
| `ansible-vault edit secret.yml`                  | Edits an existing encrypted file.                                                                                      |
| `ansible-playbook playbook.yml --ask-vault-pass` | Runs a playbook that uses encrypted files, prompting for the vault password.                                           |
| `ansible-doc <module_name>`                      | Displays documentation for a specific module (e.g., `ansible-doc copy`).                                               |
| `ansible-galaxy install <role>`                  | Installs a role from Ansible Galaxy (e.g., `ansible-galaxy install geerlingguy.apache`).                               |
| `ansible <host/group> -m setup`                  | Gathers facts about the specified hosts (useful for debugging or variable discovery).                                  |

**Tips for Commands:**

* Use `-u <user>` to specify a remote user (e.g., `ansible all -m ping -u ubuntu`).
* Use `-k` to prompt for an SSH password or `-i <inventory>` to specify a custom inventory file.
* Add `-v`, `-vv`, or `-vvv` for verbose output to debug issues.

***

### Sample Ansible Files

#### Inventory File

**Purpose**: Defines the hosts and groups Ansible manages, including connection details (e.g., IP, hostname, SSH user).

**File**: `inventory.yml`

```yaml
# inventory.yml
# Defines hosts and groups for Ansible to manage
all:
  hosts:
    web01:
      ansible_host: 192.168.1.10
      ansible_user: ubuntu
      ansible_ssh_private_key_file: ~/.ssh/id_rsa
    db01:
      ansible_host: 192.168.1.20
      ansible_user: ec2-user
  children:
    webservers:
      hosts:
        web01:
    dbservers:
      hosts:
        db01:
  vars:
    ansible_python_interpreter: /usr/bin/python3
```

**Explanation**:

* `all`: Default group containing all hosts.
* `hosts`: Individual hosts with connection details (e.g., IP, SSH user, key file).
* `children`: Subgroups for organizing hosts (e.g., `webservers`, `dbservers`).
* `vars`: Variables applied to all hosts in the group (e.g., Python interpreter path).

***

#### ansible.cfg

**Purpose**: Configures Ansible’s default settings, such as inventory location, SSH settings, or module paths.

**File**: `ansible.cfg`

```ini
# ansible.cfg
# Configuration file for Ansible settings
[defaults]
inventory = ./inventory.yml
remote_user = ubuntu
private_key_file = ~/.ssh/id_rsa
host_key_checking = False
retry_files_enabled = False
log_path = ./ansible.log

[privilege_escalation]
become = True
become_method = sudo
become_user = root
become_ask_pass = False
```

**Explanation**:

* `[defaults]`: General settings like inventory file path, default remote user, and SSH key.
* `host_key_checking = False`: Disables SSH host key checking (useful for dynamic environments).
* `[privilege_escalation]`: Configures sudo settings for tasks requiring elevated privileges.
* Place in the project directory or `/etc/ansible/ansible.cfg`.

***

#### General Playbook

**Purpose**: Defines a set of tasks to be executed on specified hosts. Playbooks are the core of Ansible automation.

**File**: `site.yml`

```yaml
# site.yml
# Main playbook to configure web and database servers
---
- name: Configure web servers
  hosts: webservers
  become: true
  tasks:
    - name: Install Apache
      apt:
        name: apache2
        state: present
        update_cache: yes
      when: ansible_os_family == "Debian"

    - name: Ensure Apache is running
      service:
        name: apache2
        state: started
        enabled: yes

- name: Configure database servers
  hosts: dbservers
  become: true
  tasks:
    - name: Install MariaDB
      apt:
        name: mariadb-server
        state: present
        update_cache: yes
      when: ansible_os_family == "Debian"

    - name: Ensure MariaDB is running
      service:
        name: mariadb
        state: started
        enabled: yes
```

**Explanation**:

* `name`: Descriptive name for the play or task.
* `hosts`: Target host group (e.g., `webservers`, `dbservers`).
* `become: true`: Enables sudo for the play.
* `tasks`: List of actions (e.g., installing packages, starting services).
* `when`: Conditional execution based on facts (e.g., OS family).
* Run with `ansible-playbook site.yml`.

***

#### Role Structure

**Purpose**: Roles organize tasks, variables, templates, and files for reusable, modular automation.

**Directory Structure**:

```
my_role/
├── defaults/
│   └── main.yml        # Default variables
├── tasks/
│   └── main.yml        # Main task list
├── templates/
│   └── config.j2       # Jinja2 template files
├── vars/
│   └── main.yml        # Role-specific variables
└── handlers/
    └── main.yml        # Handlers for event-driven tasks
```

**Sample Role File**: `roles/my_role/tasks/main.yml`

```yaml
# roles/my_role/tasks/main.yml
# Tasks for configuring a web server
---
- name: Install nginx
  apt:
    name: nginx
    state: present
    update_cache: yes

- name: Copy nginx configuration
  template:
    src: config.j2
    dest: /etc/nginx/nginx.conf
  notify: Restart nginx

- name: Ensure nginx is running
  service:
    name: nginx
    state: started
    enabled: yes
```

**Sample Handler**: `roles/my_role/handlers/main.yml`

```yaml
# roles/my_role/handlers/main.yml
# Handlers for the role
---
- name: Restart nginx
  service:
    name: nginx
    state: restarted
```

**Sample Template**: `roles/my_role/templates/config.j2`

```jinja
# roles/my_role/templates/config.j2
# Nginx configuration template
server {
    listen 80;
    server_name {{ ansible_fqdn }};
    root /var/www/html;
}
```

**Explanation**:

* `defaults/main.yml`: Low-priority variables that can be overridden.
* `tasks/main.yml`: Core tasks for the role (e.g., installing nginx, copying configs).
* `templates/`: Jinja2 templates for dynamic configuration files.
* `handlers/`: Tasks triggered by `notify` (e.g., restarting a service).
* `vars/`: High-priority variables specific to the role.
* Use roles in a playbook: `roles: - my_role`.

***

### Key Concepts

* **Inventory**: Defines hosts and groups. Can be static (YAML/INI) or dynamic (scripts or cloud plugins).
* **Playbooks**: YAML files defining automation workflows with plays and tasks.
* **Modules**: Reusable units of work (e.g., `apt`, `service`, `copy`). See `ansible-doc` for details.
* **Roles**: Modular way to organize tasks, variables, and templates for reuse.
* **Facts**: System information gathered from hosts (e.g., `ansible_os_family`). Access with `ansible <host> -m setup`.
* **Vault**: Encrypts sensitive data (e.g., passwords) using `ansible-vault`.
* **Ad-hoc Commands**: Quick, one-off tasks using `ansible` command (e.g., `ansible all -m reboot`).
* **Idempotency**: Ansible ensures tasks only make changes if needed, preventing unnecessary modifications.

**Tips**:

* Always test playbooks with `--check` and `--diff` before running.
* Use `ansible-lint` to check playbooks for best practices.
* Store sensitive data in encrypted files with `ansible-vault`.
* Organize complex projects with roles for modularity.
