Ansible tutorial
================


# ToDo:
- Opennebula
  - VPN
  - Create vm
  - ssh connect
- Ansible
  - Setup ansible
  - Idempotent vs State mashine
  - inventory, Plays, Rolle, j2
  - Project: install postgres
  - Install ansible tower (AWX docker)
- Ansible tower
  - setup postgres-prj


ansible-playbook -i inventories/hosts dispatch.yml --limit leapp-00.fritz.box -e "fqdn=leapp-00.fritz.box" -e "platform=kvm" -e "env=DEV" -e "leapp_backend_base_url=http://lazarus.fritz.box:9090"
