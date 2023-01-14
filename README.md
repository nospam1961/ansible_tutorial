# Learning Ansible from Scratch

### **References:**
- [Learn Linux TV - Getting started with Ansible](https://www.youtube.com/playlist?list=PLT98CRl2KxKEUHie1m24-wkyHpEsa4Y70)


## Create SSH keys
*Generate with Passphrase*

`$ ssh-keygen -t ed25519 -C "my default" -f ~/.ssh/id_mydefault -N YourPassphraseGoesHere`

*Generate without Passphrase*

`$ ssh-keygen -t ed25519 -C "ansible" -f ~/.ssh/id_ansible -N ""`


## Copy IDs to the server

`$ ssh-copy-id -i ~/.ssh/id_ansible.pub XXX.XXX.XXX.XXX`

`$ ssh-copy-id -i ~/.ssh/id_ansible.pub homelab-01`

`$ ssh-copy-id -i ~/.ssh/id_ansible.pub homelab-02`

`$ ssh-copy-id -i ~/.ssh/id_ansible.pub homelab-03`

## Cache your passphrase using `ssh-agent` and create alias

`$ eval $(ssh-agent)`

*Provide your passphrase to be cached*

`ssh-add`

*Create an alias to add passphrase to the cache - can add to .bashrc*

`alias ssha='eval $(ssh-agent) && ssh-add'`

## Create `inventory` and check connection
**inventory**
```
homelab-01
homelab-02
homelab-03
```

`$ ansible all --key-file ~/.ssh/ansible -i inventory -m ping`

## Create ansible configuration file and check connection
**ansible.cfg**
```
[defaults]
inventory = inventory
private_key_file = ~/.ssh/id_ansible
```

`$ ansible all -m ping`

## Command to list all hosts
`$ ansible all --list-hosts`

```
  hosts (3):
    homelab-01
    homelab-02
    homelab-03
```

## Command to Gather Facts (perfect for debugging)

`$ ansible all -m gather_facts`

or

*Gather facts only for certain server*

`$ ansible all -m gather_facts --limit homelab-02`



