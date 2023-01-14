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

Yaml```
---
- src: https://github.com/diranetafen/ansible-role-containerized-wordpress.git
```

Then update wordpress.yml with this content
Yaml```
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

