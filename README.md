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
