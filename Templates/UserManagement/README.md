# User Management Ansible Role

## Overview

This Ansible role manages user accounts on a server, including creating users, setting up SSH keys, configuring passwords, and handling user deletion. The role can be used to automate user management tasks across multiple servers.

## Role Structure

- **playbooks/user_setup.yml**: The main playbook that includes the `user_management` role.
- **roles/user_management/**: Directory containing the role with tasks, defaults, and variables.

## Usage

To use this role, include it in your main playbook and adjust the variables as needed.

### Example Playbook

```yaml
---
- name: Setup users on the server
  hosts: all
  become: yes
  roles:
    - user_management
```

### Editable Variables

The role’s behavior is controlled by variables defined in roles/user_management/defaults/main.yml. Here is a description of each variable:

**Variables in `defaults/main.yml`**

```yaml
---
users:
  - username: batman
    groups: root,wheel
    shell: /bin/bash
    create_home: yes
    ssh_key: yes
    ssh_key_bits: 2048
    ssh_key_file: .ssh/id_rsa
    password: "rosesAreRedVioletsAreBlueThisPasswordShouldBeStrongAndLongToo"
    password_expire_min: 0
    password_expire_max: 0
    state: present # Change to 'absent' to delete the user
```

- `username` **:** The username of the account to create or manage.
- `groups` **:** Comma-separated list of groups to add the user to (e.g., root,wheel).
- `shell` **:** The default shell for the user (e.g., /bin/bash).
- `create_home` **:** Whether to create the user’s home directory (yes or no).
- `ssh_key` **:** Whether to generate an SSH key for the user (yes or no).
- `ssh_key_bits` **:** Number of bits for the SSH key if ssh_key is enabled (default is 2048).
- `ssh_key_file` **:** Path to the SSH key file if ssh_key is enabled (default is .ssh/id_rsa).
- `password` **:** The user’s password.
- `password_expire_min` **:** Minimum number of days before the password can be changed (set to 0 for immediate expiry).
- `password_expire_max` **:** Maximum number of days before the password must be changed (set to 0 for immediate expiry).
- `state` **:** State of the user account. Set to present to create the user or absent to delete the user.

## How To Run

1. **Configure Variables:** Edit `roles/user_management/defaults/main.yml` to specify the users and their settings.
2. **Execute Playbook:** Run the playbook using:
    ```yaml
    ansible-playbook playbooks/user_setup.yml
    ```

## Notes

- Ensure that Ansible is installed and properly configured on your system.
- This role assumes you have proper privileges to create and delete user accounts on the target servers.