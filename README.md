# Ansible Playground 🚀
Welcome to my Ansible playground! This repository serves as a space for me to explore and document my journey with Ansible automation. Below, I've outlined key commands, configurations, and playbooks I've experimented with.

## Getting Started 🛠️

### Installation
Ensure that Ansible is installed on your machine. You can install it using:
```BASH
pip install ansible
```

### Inventory 🗂️
Define your hosts in the `inventory` file, specifying their connection details.

## Key Commands and Configurations ⚙️

### First Command 

Test SSH connection to all hosts in the inventory file:
```BASH
ansible all --key-file ~/.ssh/Master_key -i inventory -m ping
```

This command checks the connectivity to all hosts defined in the inventory file. The ping module is used here to verify the ability to connect to each host.


### Config File 

If a local `ansible.cfg` file exists, Ansible will prioritize it over the `/etc/ansible.cfg` file. With a local config, the command can be shortened to:

```BASH 
ansible all -m ping
```

Having a local ansible.cfg file allows you to set default configurations for your Ansible project, reducing the need for long command lines.


### List Hosts Commad

```BASH
ansible all --list-hosts
```

This command provides a list of hosts defined in the inventory file. It helps you verify the correctness of your inventory setup.


### Become Sudo and Run APT Update

```BASH 
ansible all -m apt -a update_cache=true --become --ask-become-pass
```

In this example, the `apt` module is used to update the package cache on all hosts. The `--become` flag is used to escalate privileges, and `--ask-become-pass` prompts for the sudo password.

### Install Package with APT Module

Install a package (e.g., Vim) using the APT module:

```BASH
ansible all -m apt -a name=vim --become --ask-become-pass
```
This command installs the specified package (`vim` in this case) on all hosts using the `apt` module. The `--become` flag is used to become sudo, and `--ask-become-pass` prompts for the sudo password.


### Package Module

The `package` module is a more generic module that supports multiple package managers, including APT. It provides a consistent interface for managing packages across different systems.

Install Package with Package Module
Install a package (e.g., Vim) using the `package` module:

```BASH
ansible all -m package -a "name=vim state=present" --become --ask-become-pass
```

This command installs the specified package (`vim` in this case) on all hosts using the `package` module. The `state=present` parameter ensures the package is present. The `--become` flag is used to become sudo, and `--ask-become-pass` prompts for the sudo password.

Difference Between APT and Package Module
The primary difference between the `apt` module and the `package` module is that the `package` module is more generic and can be used with different package managers. The `apt` module is specific to APT package management on Debian-based systems.

### Upgrade package 

Upgrade a specific package:
```BASH
ansible all -m apt -a "name=udev state=latest" --become --ask-become-pass
```

This example upgrades the `udev` package to the latest version on all hosts using the `apt` module. The `--become` flag is used for privilege escalation, and `--ask-become-pass` prompts for the sudo password.

### Upgrade all packages 
Upgrade all packages to the latest version:
```BASH
ansible all -m apt -a "upgrade=dist" --become --ask-become-pass
```

This command upgrades all packages to the latest version using the apt module with the `upgrade=dist` parameter. The `--become` flag is used for privilege escalation, and `--ask-become-pass` prompts for the sudo password.


## Playbooks 

Playbooks in Ansible are written in YAML syntax and serve as a powerful tool for orchestrating complex automation workflows. They allow you to define a series of tasks, organize them logically, and apply them to a set of hosts. Here are some key concepts related to playbooks:

### Anatomy of a Playbook
A playbook consists of one or more plays, and each play contains a set of tasks. Each task specifies an action to be performed, such as installing a package, copying a file, or restarting a service.

Here is a simple example playbook named `install_apache.yml`:
```YAML
---
- name: Install Apache
  hosts: web_servers
  become: true

  tasks:
    - name: Update APT package cache
      apt:
        update_cache: yes

    - name: Install Apache2 package
      apt:
        name: apache2
        state: present
```
In this playbook:

* `name`: A user-friendly description of the playbook.
* `hosts`: Specifies the target hosts or groups of hosts on which the playbook should run.
* `become`: Allows tasks to run with elevated privileges (sudo).

The tasks section contains a list of actions to be executed on the specified hosts. In this example, the playbook updates the APT package cache and installs the Apache2 package.

### Running Playbooks
To execute a playbook, use the ansible-playbook command followed by the playbook filename. For example:

```BASH
ansible-playbook --ask-become-pass install_apache.yml
```

### Roles
Roles provide a way to organize playbooks and share functionalities across multiple playbooks. They encapsulate tasks, handlers, variables, and files into a structured directory format.

Here's a simplified directory structure for a role named `web_server`:
```
web_server/
├── tasks/
│   └── main.yml
├── handlers/
│   └── main.yml
├── vars/
│   └── main.yml
├── files/
│   └── ...
└── meta/
    └── main.yml
```
* `tasks`: Contains the main tasks to be executed.
* `handlers`: Includes handlers, which are tasks triggered by other tasks.
* `vars`: Defines variables used within the role.
* `files`: Holds files that are copied to the target hosts.
* `meta`: Specifies metadata about the role.

### Templating
Ansible uses Jinja2 templating to dynamically generate configurations. This allows you to create flexible and reusable playbooks by incorporating variables and conditionals.

For example, consider a template for an Apache virtual host configuration:
```
<VirtualHost *:80>
    ServerName {{ server_name }}
    DocumentRoot /var/www/{{ app_name }}
</VirtualHost>
```
Variables such as `server_name` and `app_name` can be defined in the playbook or inventory, allowing for customization.

### Conditional Execution
Tasks can be executed conditionally based on specific criteria. This feature enables you to handle different scenarios on different hosts.

Here's an example:
```YAML
- name: Ensure Apache is installed on Debian systems
  apt:
    name: apache2
    state: present
  when: ansible_distribution == 'Debian'
```

In this task, Apache is installed only if the target host is running a Debian distribution.

Playbooks, roles, templating, and conditional execution are powerful concepts that empower you to automate and manage infrastructure efficiently. Explore and customize these features to suit the needs of your Ansible projects!

***
Feel free to experiment with these commands and adapt them to your Ansible projects! Happy automating! 🎉
