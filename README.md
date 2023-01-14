# Learning Ansible from Scratch

### **References:**
- [Learn Linux TV - Getting started with Ansible](https://www.youtube.com/playlist?list=PLT98CRl2KxKEUHie1m24-wkyHpEsa4Y70)


## Starting with 3 Ubuntu servers and workstation (via  WSL2) - DNS names can be replaced with IP addresses
```
homelab-01
homelab-02
homelab-03
```


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


## Running commands on the remote boxes

### Run apt update on the remote servers as sudo (on debian based )
```
- -m -> module
- -a -> argument
- --become -> run as sudo user (elevate privileges)
- --ask-become-pass -> prompt for the password (sudo password on workstation and servers should be the same)
```

`$ ansible all -m apt -a update_cache=yes --become --ask-become-pass`

### Install package on all remote servers

`$ ansible all -m apt -a name=vim-nox --become --ask-become-pass`

`$ ansible all -m apt -a name=tmux --become --ask-become-pass`

### Check existence of the package on remote

`$ apt search vim-nox`

### Log of apt on remote

`$ cat /var/log/apt/history.log`

### Upgrade package

** This shows that package is installed previously (green output with "SUCCESS") **

`ansible all -m apt -a name=ssh --become --ask-become-pass`

** Upgrade the package to the latest by passing another argument**

`ansible all -m apt -a "name=ssh state=latest" --become --ask-become-pass`


### Upgrade to latest distribution all packages on the remotes

`ansible all -m apt -a "upgrade=dist" --become --ask-become-pass`


## First Playbook

### Install apache2 package

`$ ansible-playbook --ask-become-pass install_apache.yml`