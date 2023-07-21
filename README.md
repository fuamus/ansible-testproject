# Ansible tutorial
### versions/tools
Centos 7  
OpenNebula

### Cluster
f-test-01.fritz.box: postgres  
f-test-03.fritz.box: Ansible, Ansible Tower

---
## Table of Contents
1. Useful ansible commands
2. Setup Centos 7 VM with OpenNebula
3. Install Ansible Tower on Centos 7
---
## 1. Useful ansible commands
````bash
ansible-tower-service status

ansible all -i inventories/psql --list-hosts

# ansible-vault
ansible-playbook  -i inventories/psql playbooks/demo.yml --ask-vault-pass -e "@~/app.json"
ansible-vault create $HOME/app.json
ansible-vault edit $HOME/app.json
````
## 2. Setup VM
### Create VM
Create new VM by going under instances on OpenNebula ....

### Configure VM
**initial login**  
login: root  
password: Dilbert4Dilbert

**optional: change system keyboard keymap layout**
```bash
root@hostname:~# loadkeys de # for German
# permanently change keymap
root@hostname:~# localectl set-keymap de
```

**ssh configs anpassen**  

```bash
root@hostname:~# vi /etc/ssh/sshd_config 
# suchen nach 'Passwort'
# PasswordAuthentification auf 'yes' setzen
root@hostname:~# systemctl restart sshd
```

**hostnamen setzen**
```bash
root@hostname:~# hostnamectl set-hostname <my.new-hostname.server>
```

**repo subscription**
```bash
root@hostname:~# subscription-manager register --username=ri@multilix.com --password=<password>  --auto-attach [--force]
# if there are issues with login via CLI, following commands might help:
root@hostname:~# subscription-manager remove --all
root@hostname:~# subscription-manager unregister
root@hostname:~# subscription-manager clean
root@hostname:~# yum clean all
root@hostname:~# rm -rf /var/cache/yum/*
root@hostname:~# subscription-manager register
root@hostname:~# subscription-manager --username=<username> --password=<password> attach --auto --force
root@hostname:~# subscription-manager status
```

**applying package updates**
````bash
root@hostname:~# yum -y update
````

**optional: Customize Bash Prompt**
```bash
root@hostname:~# vi /etc/bashrc
# Comment out the default settings
[ "$PS1" = "\\s-\\v\\\$ " ] && PS1="[\u@\h \w]\\$ "
# and add your customization below
PS1='\u@\H:\w\$ '
# user@hostname.domain.tld:/working/directory$
``` 

**user anlegen**
```bash
root@hostname:~# adduser <username>
root@hostname:~# passwd <username>
```

**Add User to Sudoers**
````bash
root@hostname:~# visudo
````

add the following line  
`username  ALL=(ALL) NOPASSWD:ALL`  
below the end of the following:
```bash
## Allows people in group wheel to run all commands
%wheel  ALL=(ALL)       ALL

## Same thing without a password
# %wheel        ALL=(ALL)       NOPASSWD: ALL
username  ALL=(ALL) NOPASSWD:ALL
```

**Copy the key to a server**  
`ssh-copy-id -i ~/.ssh/mykey user@host`
---
## 2. Installing Ansible Tower
Source: https://computingforgeeks.com/install-and-configure-ansible-tower-on-centos-rhel/?expand_article=1
#### Update system and add repos
```bash
[ansible@hostname ~]$ sudo yum update -y

[ansible@hostname ~]$ sudo yum install -y epel-release
# epel-release package is directly available in CentOS base repository, but not in RHEL repository
[ansible@hostname ~]$ sudo yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm

[ansible@hostname ~]$ sudo yum repolist

[ansible@hostname ~]$ sudo yum install -y ansible

[ansible@hostname ~]$ sudo yum update -y ansible
```
#### Download & extract Ansible Tower archive
````bash
# root@hostname:~# mkdir /tmp/tower && cd  /tmp/tower
root@hostname:~# curl -k -O https://releases.ansible.com/ansible-tower/setup/ansible-tower-setup-latest.tar.gz

root@hostname:~# tar xvf ansible-tower-setup-latest.tar.gz
````
#### Install Ansible Tower
````bash
root@hostname:~# cd ansible-tower-setup*/

root@hostname:~# vim inventory
...........................
[tower]
localhost ansible_connection=local

[database]

[all:vars]
admin_password='AdminPassword'

pg_host=''
pg_port=''

pg_database='awx'
pg_username='awx'
pg_password='PgStrongPassword'

rabbitmq_username=tower
rabbitmq_password='RabbitmqPassword'
rabbitmq_cookie=cookiemonster

# Isolated Tower nodes automatically generate an RSA key for authentication;
# To disable this behavior, set this value to false
# isolated_key_generation=true

root@hostname:~# sudo ./setup.sh
````


# Run ansible roles
```shell
ans -lkkdaskds
```

# Verfiy





```bash
ansible-playbook -i inventories/hosts dispatch.yml --limit leapp-00.fritz.box -e "fqdn=leapp-00.fritz.box" -e "platform=kvm" -e "env=DEV" -e "leapp_backend_base_url=http://lazarus.fritz.box:9090"
```
