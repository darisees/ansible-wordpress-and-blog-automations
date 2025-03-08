# Getting Started

Welcome to the Ansible Playbook Repository for Automating WordPress and Blog Setup. This repository contains two playbooks designed to streamline the deployment process of WordPress and a blog system using Ansible.

## Folder Structure

The repository contains the following structure:

- `wordpress.yml` : Playbook for installing and configuring WordPress.
- `blog.yml` : Playbook for setting up a blog system.
- `inventory` : File that defines the target hosts for Ansible.
- `docs/` : Documentation related to the playbooks and setup process.

## Playbook Descriptions

### WordPress Playbook
This playbook automates the installation and configuration of a WordPress website, including:

- Installing Apache, MySQL, and PHP.
- Setting up a MySQL database for WordPress.
- Downloading and configuring WordPress.
- Adjusting Apache settings for proper deployment.

### Blog Playbook
This playbook is used for setting up a simple blog system with similar automation principles. It includes:

- Web server installation.
- Database setup.
- Application deployment.

## How to Use

1. Ensure you have Ansible installed.
2. Update the `inventory` file with your server details.
3. Run the playbook:
   ```sh
   ansible-playbook -i inventory wordpress.yml
   ```
   or
   ```sh
   ansible-playbook -i inventory blog.yml
   ```
4. Verify the installation by accessing the web server URL.

For more details, check the documentation in the `docs/` folder.
