# Ansible
My Ansible playground

### First Command 

```BASH
ansible all --key-file ~/.ssh/Master_key -i inventory -m ping
```

this command will test all hosts in inventory file and try to make SSH Connection With them.


### Config File 

if whe have a local `ansible.cfg` ansible will read the default configs and ignore the `/etc/ansible.cfg` config file.
then we can shorten our command to this : 

```BASH 
ansible all -m ping
```


### List hosts Commad

```BASH
root@master:/home/zero/Ansible# ansible all --list-hosts
  hosts (3):
    node-1.zeroCluster.lab
    node-2.zeroCluster.lab
    node-3.zeroCluster.lab
```
with this command ansible will list all hosts available.


### Become Sudo and Run APT

```BASH 
root@master:/home/zero/Ansible# ansible all -m apt -a update_cache=true --become --ask-become-pass
BECOME password:
node-1.zeroCluster.lab | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "cache_update_time": 1705781109,
    "cache_updated": false,
    "changed": false
}
node-2.zeroCluster.lab | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "cache_update_time": 1705781411,
    "cache_updated": false,
    "changed": false
}
node-3.zeroCluster.lab | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "cache_update_time": 1705785237,
    "cache_updated": false,
    "changed": false
}
```

### Install package with apt madule
```BASH
root@master:/home/zero/Ansible# ansible all -m apt -a name=vim --become --ask-become-pass
```


### Upgrade package 
```BASH
ansible all -m apt -a "name=udev state=latest" --become --ask-become-pass
```

### Upgrade all packages 
```BASH
ansible all -m apt -a "upgrade=dist" --become --ask-become-pass
```


## playbooks 

playbooks are in yaml syntax for example `install_apache.yml`

### run playbooks in ansible 

```BASH
root@master:/home/zero/Ansible# ansible-playbook --ask-become-pass install_apache.yml
BECOME password:

PLAY [all] ************************************************************************************************************************************************

TASK [Gathering Facts] ************************************************************************************************************************************
ok: [node-1.zeroCluster.lab]
ok: [node-3.zeroCluster.lab]
ok: [node-2.zeroCluster.lab]

TASK [install apache2 package] ****************************************************************************************************************************
changed: [node-3.zeroCluster.lab]
changed: [node-2.zeroCluster.lab]
changed: [node-1.zeroCluster.lab]

PLAY RECAP ************************************************************************************************************************************************
node-1.zeroCluster.lab     : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
node-2.zeroCluster.lab     : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
node-3.zeroCluster.lab     : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```
