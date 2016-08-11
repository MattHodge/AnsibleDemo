# vagrant ssh control

```
cd ~/ansible
ansible web -m win_ping
ansible web -m raw -a "ipconfig"
ansible web -m win_service -a "name=spooler"
ansible web -m win_service -a "name=spooler state=stopped"
ansible-playbook webservers.yml
ansible-vault encrypt group_vars/web.yml
ansible-playbook webservers.yml --ask-vault-pass
```
