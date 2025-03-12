# Job Sheet: Automating Blog Deployment with Ansible

This job sheet will guide you through the process of setting up an automated deployment for a blog application using Ansible. By the end of this guide, you will have a fully functional blog hosted on an Apache server with a MySQL database.

### Steps Covered:
1. Updating the package repository.
2. Installing required software packages (Apache, MariaDB, PHP).
3. Configuring MySQL database and user.
4. Cloning the blog application from a Git repository.
5. Importing the database schema.
6. Restarting services to apply changes.

## Step 1: Preparing Your Environment

### Exercise 1.1: Creating the Inventory File
Create an inventory file named `inventory.ini` with the following content:

```ini
[blog]
your_host_ip ansible_user=your_host_user ansible_ssh_private_key_file=/path/to/private/key.pem
```

- **Objective:** Define the server’s IP, host user, and SSH key.
- **Additional Exercise:** Modify the file to connect to a different server, if available.

### Exercise 1.2: Testing Ansible Connectivity
Verify the Ansible connection using the command:

```sh
ansible -i inventory.ini blog -m ping
```

- **Objective:** Ensure Ansible can connect to the server.
- **Additional Exercise:** Use a different inventory or modify the connection parameters.

## Step 2: Playbook Breakdown

### Host Definition & Variables
```yaml
- hosts: blog
  become: yes
  vars:
    mysql_db_name: cyber23
    mysql_user: cyber
    mysql_user_pass: pass2023
    mysql_root_pass: 19091Cyber
    git_repo: "https://github.com/jonisetiyawan48/cyber_ukk.git"
    web_root: "/var/www/html"
```
- Defines the target host (`blog`).
- Uses `become: yes` to elevate privileges.
- Sets up variables for database credentials and the web root path.

### Updating the Package Repository
```yaml
  tasks:
    - name: Update package repository
      apt:
        update_cache: yes
```
- Ensures the system has the latest package information.

### Installing Apache, MariaDB, and PHP
```yaml
    - name: Install Apache, MariaDB, and PHP
      apt:
        name:
          - apache2
          - mariadb-server
          - php
          - php-mysqli
          - php-mbstring
        state: present
```
- Installs required software packages.

### Starting and Enabling Services
```yaml
    - name: Start and enable Apache
      service:
        name: apache2
        state: started
        enabled: yes
```
```yaml
    - name: Start and enable MariaDB
      service:
        name: mariadb
        state: started
        enabled: yes
```
- Ensures that Apache and MariaDB services are running and enabled on boot.

### Installing Python MySQL Dependency
```yaml
    - name: Install Python MySQL library
      apt:
        name: python3-pymysql
        state: present
```
- Installs required MySQL dependency for Ansible.

### Configuring MySQL Database and User
```yaml
    - name: Update root password for MariaDB
      mysql_user:
        name: root
        host: localhost
        password: "{{ mysql_root_pass }}"
        login_unix_socket: /var/run/mysqld/mysqld.sock
        state: present
```
```yaml
    - name: Create MySQL user
      mysql_user:
        name: "{{ mysql_user }}"
        password: "{{ mysql_user_pass }}"
        priv: "{{ mysql_db_name }}.*:ALL"
        host: "%"
        state: present
        login_user: root
        login_password: "{{ mysql_root_pass }}"
```
```yaml
    - name: Create MySQL database
      mysql_db:
        name: "{{ mysql_db_name }}"
        state: present
        login_user: root
        login_password: "{{ mysql_root_pass }}"
```
- Sets up the MySQL root password.
- Creates a dedicated MySQL user and database for the blog application.

### Cloning the Blog Repository
```yaml
    - name: Clone Git repository
      git:
        repo: "{{ git_repo }}"
        dest: "{{ web_root }}/"
        update: yes
```
- Clones the blog application from the specified GitHub repository.

### Importing the Database Schema
```yaml
    - name: Import SQL file into database
      mysql_db:
        name: "{{ mysql_db_name }}"
        state: import
        target: "{{ web_root }}/db.sql"
```
- Imports the initial database schema from the cloned repository.

### Configuring Apache for the Blog
```yaml
    - name: Enable the new site configuration
      command: a2ensite 000-default.conf
```
- Ensures the Apache configuration is set up correctly.

### Restarting Apache to Apply Changes
```yaml
    - name: Restart Apache to apply changes
      service:
        name: apache2
        state: restarted
```
- Restarts Apache to apply all changes.

## Final Practice: Full Playbook Execution
To execute the full automation, combine all tasks and run:

```sh
ansible-playbook -i inventory.ini blog.yml
```

- **Objective:** Automate the complete Blog installation and configuration process.
- **Observation:** After running the playbook, Blog should be fully installed and configured.

## Summary
This Ansible playbook automates the entire blog deployment process, from setting up required software and database to cloning the application and configuring the web server. Running this playbook will deploy a fully functional blog on an Apache server.

