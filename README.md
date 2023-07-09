# Ansible tutorial

---
## Summary
1. Prerequisites
   1. VM Setup
---
## Prerequisites
### Setup Centos 7 VM with OpenNebula
#### Create VM
Create new VM by going under instances on OpenNebula ....

#### Configure VM
**initial login**  
login: root  
password: Dilbert4Dilbert

**optional: change system keyboard keymap layout**
```bash
$# loadkeys de # for German
# permanently change keymap
$# localectl set-keymap de
Loading /lib/kbd/keymaps/xkb/sk.map.gz
```

**ssh configs anpassen**
`$# vi /etc/ssh/sshd_config`
suchen nach 'Passwort'
PasswordAuthentification auf 'no' setzen
`$# systemctl restart sshd`

**hostnamen setzen**
`$# hostnamectl set-hostname <my.new-hostname.server>`

**repo subscription**
```bash
subscription-manager register --username=ri@multilix.de --password=<password>  --auto-attach --force

# if there are issues with login via CLI, following commands might help:
$# subscription-manager remove --all
$# subscription-manager unregister
$# subscription-manager clean
$# yum clean all
$# rm -rf /var/cache/yum/*
$# subscription-manager register
$# subscription-manager --username=<username> --password=<password> attach --auto --force
$# subscription-manager status
```

**applying package updates**
`$# yum -y update`

**optional: Customize Bash Prompt**
```
$# vi /etc/bashrc
# Comment out the default settings
[ "$PS1" = "\\s-\\v\\\$ " ] && PS1="[\u@\h \w]\\$ "
# and add your customization below
PS1='\u@\H:\w\$ '
# user@hostname.domain.tld:/working/directory$
``` 

**user anlegen**
```bash
$# adduser <username>
$# passwd <username>
```

**Add User to Sudoers**
`visudo # as root`
add the following line
`username  ALL=(ALL) NOPASSWD:ALL`
below folliwing part
```bash
## Allows people in group wheel to run all commands
%wheel  ALL=(ALL)       ALL

## Same thing without a password
# %wheel        ALL=(ALL)       NOPASSWD: ALL
username  ALL=(ALL) NOPASSWD:ALL
```

**Copy the key to a server**
`ssh-copy-id -i ~/.ssh/mykey user@host`



```bash
ansible-playbook -i inventories/hosts dispatch.yml --limit leapp-00.fritz.box -e "fqdn=leapp-00.fritz.box" -e "platform=kvm" -e "env=DEV" -e "leapp_backend_base_url=http://lazarus.fritz.box:9090"
```

# Run ansible roles
```shell
ans -lkkdaskds
```

# Verfiy


