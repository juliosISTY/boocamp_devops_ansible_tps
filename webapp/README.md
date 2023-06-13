# Security notes

## encrypt with ansible-vault

To encrypt some file run and set the vault password:
> ansible-vault encrypt files/secrets/credentials.yml
Run this command to execute your plabook now:
> ansible-playbook -i prod.yml deploy.yml --ask-vault-pass

## Ansible ssh key

To generate ssh key and copy it to the remote host, run these commands below:
> ssh-keygen -t rsa
> ssh-copy-id admin@[ip_address]

# Roles Ansible

Other method to run your rule from git is to, in your rule folder add requirements.yml file with this content:
```Yaml
---
# Install a role for Wordpress
- src: https://github.com/diranetafen/ansible-role-containerized-wordpress.git
```
Then update wordpress.yml with this content
```Yaml
---
- name: "deployment of Wordpress"
  hosts: prod
  become: true
  gather_facts: yes
  vars:
    system_user: admin
  vars_files:
    - files/secrets/credentials.yml
  roles:
    - {role: ansible-role-containerized-wordpress}
  pre_tasks:
    - name: Ensure group "www-data" exists or created
      group:
      name: "www-data"
      state: present

    - name: Add wordpress user
      user:
      name: "www-data"
      comment: wordpress user
      group: "www-data"
      append: yes
 ```
Final run the command below to download the role reposiroty before to launch the playbook:
> ansible-galaxy install -r roles/requirements.yml

# Ansible Tower (AWX)

## Deploy AWX on Docker 

```sh
git clone https://github.com/diranetafen/cursus-devops.git
cd cursus-devops/tower/
tar -xzvf awx.tar.gz -C ~/
cd ~/.awx/awxcompose/
docker-compose up -d
```
**Notes:** *Port:80, Username=admin and Password=password*

## Steps or workflow to do

When you are connected to the dashboard; firstly created a new project and included the github repository, secondly created and inventory and added the source of hosts file, and then created the job that will use to run the playbook of (tp5 or tp7).
In extension, you can add a webhook to automatically run the job when you will do some commit of source code on the Github repo.

# Ansible + AWS (to do the provisioning of ec2 instance)

Go to [ansible aws ec2](https://github.com/juliosISTY/ansible-aws-ec2.git)
