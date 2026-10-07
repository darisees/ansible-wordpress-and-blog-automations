# Ansible WordPress & Blog Automation

Automated web deployment project using Ansible on AWS EC2.

## 📌 Overview

This repository contains the Ansible playbooks and project documentation for an automated web deployment project using AWS and Ansible.

The project was developed as a final examination project at SMK Telkom Malang, focusing on deploying and configuring WordPress and a blog application on AWS EC2 instances. Ansible was used to automate server and application configuration, including web server, database, PHP, and application setup.

The project also covers supporting infrastructure and deployment components, including AWS networking, Load Balancer, Auto Scaling, and basic web security testing and vulnerability patching.

## 🏗️ Architecture

The overall deployment architecture is illustrated below.

![AWS Ansible Deployment Architecture](./docs/aws-ansible-deployment-architecture.png)

The project uses AWS EC2 instances as the deployment environment, with `AnsibleNode` acting as the Ansible control node for configuring the target servers.

The infrastructure also includes AWS networking, Application Load Balancers, and Auto Scaling components to support the deployed applications.

## 🎯 Project Context

This project was completed as a hands-on final examination project at SMK Telkom Malang.

The project was carried out as a group, with each member working through the deployment workflow and gaining hands-on experience with the overall system.

The main objective was to understand how web applications can be deployed and configured on cloud infrastructure while using automation to simplify server configuration and application setup.

The deployment environment used AWS EC2 instances running Ubuntu, with one instance used as the Ansible control node and separate target instances for the WordPress and blog applications.

## 🛠️ Technologies

- **AWS EC2** — Cloud compute instances
- **AWS VPC** — Network infrastructure
- **AWS Application Load Balancer** — Traffic distribution
- **AWS Auto Scaling** — Instance scaling
- **Ubuntu 24.04.2 LTS** — Server operating system
- **Ansible** — Server and application automation
- **Apache** — Web server
- **MySQL / MariaDB** — Database
- **PHP** — Application runtime
- **SSH** — Remote server access
- **Git** — Application source management

## 📁 Repository Structure

```text
.
├── docs/
│   ├── aws-ansible-deployment-architecture.png
│   └── Automate-Web-Deployment-with-Ansible-on-AWS.pdf
│
├── job-sheet/
│   ├── Job Sheet Automating Blog.md
│   ├── Job Sheet Automating WordPress.md
│   └── README.md
│
├── playbooks/
│   ├── wordpress.yml
│   ├── wordpress.yml
│   └── README.md
│
├── LICENSE
│
└── README.md
```

### `docs/`

Contains the project architecture diagram and the original project documentation.

### `job-sheet/`

Contains supporting job sheets and materials used during the project implementation.

### `playbooks/`

Contains the Ansible playbooks used to automate the deployment and configuration of the target applications.

## ⚙️ Ansible Playbooks

### WordPress — `wordpress.yml`

The WordPress playbook automates the configuration required to deploy a WordPress application, including:

- Updating the package repository
- Installing Apache, MySQL, and PHP
- Configuring the WordPress database
- Downloading and configuring WordPress
- Configuring the Apache web server
- Applying the required application configuration

### Blog — `blog.yml`

The blog playbook automates the configuration of the blog application, including:

- Updating the package repository
- Installing Apache, MariaDB, and PHP
- Configuring the application database
- Cloning the blog application from a Git repository
- Configuring the web server
- Importing the required SQL data

## 🚀 Deployment Workflow

The deployment process was carried out through several stages.

### 1. AWS Infrastructure Setup

The initial infrastructure was prepared on AWS, including:

- VPC and subnet configuration
- Network routing
- Security groups
- EC2 instances

The project used three main EC2 roles:

- `AnsibleNode` — Ansible control node
- `TargetCMS` — WordPress server
- `TargetBlog` — Blog application server

### 2. AnsibleNode Configuration

The Ansible control node was accessed through SSH and prepared for automation.

The system was updated and upgraded, followed by the installation and configuration of Ansible.

### 3. Inventory Configuration

The target servers were defined in an Ansible inventory file.

The inventory grouped the target servers into WordPress and blog hosts.

Example:

```ini
[wordpress]
<wordpress-server>

[blog]
<blog-server>
```

SSH configuration was then used to allow Ansible to connect to the target instances.

### 4. Connectivity Testing

Before running the playbooks, connectivity between the Ansible control node and target servers was tested using the Ansible ping module.

```bash
ansible -i inventory.ini wordpress -m ping
ansible -i inventory.ini blog -m ping
```

A successful connection returned a `pong` response from the target server.

### 5. Application Deployment

After the target servers were reachable, the corresponding playbooks were executed.

For WordPress:

```bash
ansible-playbook -i inventory.ini playbooks/wordpress.yml
```

For the blog application:

```bash
ansible-playbook -i inventory.ini playbooks/blog.yml
```

The playbooks performed the required server and application configuration automatically.

### 6. Load Balancer Configuration

After the applications were deployed, Application Load Balancers were configured to provide access to the deployed applications.

Target groups were configured for the EC2 instances and HTTP traffic was handled through port 80.

### 7. Auto Scaling Configuration

The WordPress infrastructure was also configured with an Auto Scaling Group.

The workflow included creating an AMI, preparing a Launch Template, and configuring an Auto Scaling Group for the WordPress environment.

This allowed the deployment to be tested in an automatically managed EC2 environment.

## 🔐 Security Testing & Patching

The project also included basic web security testing on the deployed blog application.

Directory enumeration was performed to identify accessible endpoints. An SQL injection vulnerability was then identified in the application's login functionality.

The vulnerable source code was analyzed and patched using prepared statements to prevent SQL injection.

After the patch was applied, the SQL injection test was repeated and the vulnerability was no longer exploitable.

This part of the project provided hands-on experience with basic vulnerability identification, source-code analysis, and application-level security patching.

## 📚 Documentation

The repository includes the original project documentation from the implementation phase.

The documentation provides additional details and screenshots covering:

- AWS infrastructure setup
- VPC and networking configuration
- EC2 deployment
- AnsibleNode setup
- Ansible inventory
- Playbook development and execution
- Load Balancer configuration
- Auto Scaling configuration
- Security testing
- Vulnerability patching

For the complete implementation documentation:

[View Project Documentation](./docs/Automate-Web-Deployment-with-Ansible-on-AWS.pdf)

## ❓ How to Use

### Prerequisites

- Ansible
- Linux environment
- Accessible target server(s)
- SSH access to the target server(s)
- Configured Ansible inventory
- SSH authentication credentials

### 1. Clone the Repository

```bash
git clone https://github.com/darisees/ansible-wordpress-and-blog-automations.git
cd ansible-wordpress-and-blog-automations
```

### 2. Configure the Inventory

Create or update `inventory.ini` with the target server information.

Example:

```ini
[wordpress]
<wordpress-ip> ansible_user=ubuntu ansible_ssh_private_key_file=/path/to/key.pem

[blog]
<blog-ip> ansible_user=ubuntu ansible_ssh_private_key_file=/path/to/key.pem
```

### 3. Test Connectivity

Test the WordPress server:

```bash
ansible -i inventory.ini wordpress -m ping
```

Test the blog server:

```bash
ansible -i inventory.ini blog -m ping
```

Make sure the target servers return a successful `pong` response.

### 4. Run the Playbooks

Deploy WordPress:

```bash
ansible-playbook -i inventory.ini wordpress.yml
```

Deploy the blog application:

```bash
ansible-playbook -i inventory.ini blog.yml
```

The playbooks will perform the required server and application configuration according to their tasks.

## Notes

This repository represents a completed academic project and is intended primarily as a reference and portfolio of the implementation process.

The original AWS environment used for the project is no longer active. Therefore, the IP addresses, DNS records, and AWS resources shown in the documentation are historical and are not intended for current deployment.

The original inventory, SSH private key, and project-specific credentials are not included in this repository.

The playbooks may require adjustments to the inventory, credentials, application sources, and infrastructure configuration before being used in a new environment.
