# boocamp_devops_ansible_tps

## Commandes ADD-DOC:

Ping command:
> ansible -i inventaire.ini all -m ping

***NB: skip finger print step with ansible when you are doing first ssh ping (faire confiance à l'hôte distant): ansible_ssh_common_args='-o StrictHostKeyChecking=no'***

Create file:
> ansible -i inventaire.ini all -m copy -a "dest=/home/admin/toto.txt content='Bonjour eazytraining'"

> ansible -i inventaire.ini all -m copy -a "src=toto.txt dest=/home/admin/"

Have all details about your remote nodes:
> ansible -i inventaire.ini all -m setup

Examples of inventory.yml and inventory.ini

```Yaml
---
all:
  hosts:
    client1:
      ansible_host: 10.0.4.4
    client2:
      ansible_host: 10.0.4.5
  vars:
    ansible_user: "admin"
    ansible_password: "admin"
    ansible_ssh_common_args='-o StrictHostKeyChecking=no'
    
---
all:
  hosts:
    10.0.4.4
    10.0.4.5
  vars:
    ansible_user: "admin"
    ansible_password: "admin"
    ansible_ssh_common_args='-o StrictHostKeyChecking=no'
    
---
all:
  vars:
    ansible_user: "admin"
    ansible_ssh_common_args='-o StrictHostKeyChecking=no'

prod:
  hosts:
    10.0.4.4
  vars:
    ansible_password: "admin"
    env: production
```

```Ini
client1 ansible_host=10.0.4.4 ansible_user=admin ansible_password=admin ansible_ssh_common_args='-o StrictHostKeyChecking=no'
client2 ansible_host=10.0.4.5 ansible_user=admin ansible_password=admin ansible_ssh_common_args='-o StrictHostKeyChecking=no'

---
[all:vars]
ansible_user=admin
ansible_ssh_common_args='-o StrictHostKeyChecking=no'

[prod]
10.0.4.4

[prod:vars]
ansible_password=admin
env=production
```
Use variable:
> ansible -i inventaire.ini all -m copy -a "dest=/home/admin/toto.txt content='Bonjour eazytraining {{env}}'"
