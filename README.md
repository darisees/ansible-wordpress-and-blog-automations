# Ansible WordPress & Blog Automation

Welcome to the Ansible Playbook Repository for automating the deployment of WordPress and a blog system. This repository is structured to help users easily set up and manage these applications on a server using Ansible.

## 📁 Folder Structure

The repository contains the following directories and files:

- **`job-sheet/`** : Contains job sheets that explain the automation process in detail.
- **`playbooks/`** : Stores the Ansible playbooks for deploying WordPress and the blog application.
- **`LICENSE`** : The license file for this repository.
- **`README.md`** : This document, providing an overview of the repository.

## 📜 Playbook Descriptions

### WordPress Playbook (`playbooks/wordpress.yml`)
Automates the installation and configuration of a WordPress website, including:

- Installing Apache, MySQL, and PHP.
- Creating and configuring a MySQL database for WordPress.
- Downloading and setting up WordPress.
- Configuring Apache for WordPress deployment.

### Blog Playbook (`playbooks/blog.yml`)
Automates the setup of a blog application with similar principles:

- Installing required packages (Apache, MariaDB, PHP).
- Setting up the MySQL database and user.
- Cloning a blog application from a Git repository.
- Configuring the web server for proper deployment.

## 🚀 How to Use

1. Ensure Ansible is installed on your system.
2. Update the `inventory.ini` file with your server details.
3. Run one of the following commands:

   ```sh
   ansible-playbook -i inventory.ini playbooks/wordpress.yml
   ```
   or
   ```sh
   ansible-playbook -i inventory.ini playbooks/blog.yml
   ```
4. Access the deployed application via your web browser

For detailed explanations, refer to the documentation in the **`job-sheet/`** folder.

Feel free to contribute to improve these playbooks or optimize the configurations!
