# Ansible Playbooks

This directory contains Ansible playbooks used to automate the deployment and configuration of WordPress and a blog application.

## Playbooks

### `wordpress.yml`

Automates the deployment and configuration of WordPress, including:

- Installing Apache, MySQL, and PHP
- Configuring the WordPress database
- Downloading and configuring WordPress
- Configuring the Apache web server

### `blog.yml`

Automates the deployment and configuration of the blog application, including:

- Installing Apache, MariaDB, and PHP
- Configuring the application database
- Cloning the blog application from a Git repository
- Configuring the Apache web server
- Importing the required SQL data

## How to Run

Before running the playbooks, make sure you have:

- Ansible installed
- An inventory file configured with the target server details
- SSH access to the target servers

From the repository root, run:

```bash
ansible-playbook -i inventory.ini wordpress.yml
```

For the blog application:

```bash
ansible-playbook -i inventory.ini blog.yml
```

The playbooks require a properly configured inventory and SSH credentials for the target servers.
