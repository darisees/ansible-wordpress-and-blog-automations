# Ansible Playbooks

This folder contains Ansible playbooks for automating the deployment of WordPress and a blog application.  

## Playbooks Available
1. **`wordpress.yml`**  
   - Automates the installation and configuration of WordPress on an Apache server.  
   - Sets up the required MySQL database, installs necessary packages, and configures Apache.  

2. **`blog.yml`**  
   - Automates the deployment of a blog application.  
   - Installs required dependencies, sets up the database, clones the blog repository, and configures Apache.  

## How to Run
Before running the playbooks, make sure you have:  
- Ansible installed on your local machine.  
- An inventory file (`inventory.ini`) with the target server details.  
- SSH access to the target machine.  

Run the playbooks using the following command:  

```sh
ansible-playbook -i inventory.ini wordpress.yml
```
or
```sh
ansible-playbook -i inventory.ini blog.yml
```

## Contribution
Feel free to contribute by improving the playbooks, adding new automation features, or optimizing configurations.
