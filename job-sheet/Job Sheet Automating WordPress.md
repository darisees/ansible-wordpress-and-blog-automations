# Job Sheet: Automating WordPress Installation with Ansible

This job sheet will guide you through the process of setting up a fully automated WordPress installation using Ansible. By the end of this guide, you will have a functioning WordPress site installed on an Apache server with a MySQL database.

### Steps Covered:
1. Preparing your environment.
2. Updating the package repository.
3. Installing required software packages (Apache, MySQL, PHP).
4. Setting up a MySQL database and user.
5. Downloading and configuring WordPress.
6. Adjusting Apache configurations for WordPress.
7. Restarting services to apply changes.

## Step 1: Preparing Your Environment

### Exercise 1.1: Creating the Inventory File
Create an inventory file named `inventory.ini` with the following content:

```ini
[wordpress]
your_host_ip ansible_user=your_host_user ansible_ssh_private_key_file=/path/to/private/key.pem
```

- **Objective:** Define the server’s IP, host user, and SSH key.
- **Additional Exercise:** Modify the file to connect to a different server, if available.

### Exercise 1.2: Testing Ansible Connectivity
Verify the Ansible connection using the command:

```sh
ansible -i inventory.ini wordpress -m ping
```

- **Objective:** Ensure Ansible can connect to the server.
- **Additional Exercise:** Use a different inventory or modify the connection parameters.

## Step 2: Playbook Breakdown

### Host Definition & Variables
```yaml
- hosts: wordpress
  become: yes
  vars:
    wp_db_name: wordpress
    wp_user: wordpress
    wp_user_pass: wordpress
    wp_root_pass: 123wordpress
    web_root: /var/www/wordpress
```
- Defines the target host (`wordpress`).
- Elevates privileges (`become: yes`).
- Sets up variables for database credentials and the web root path.

### Updating the Package Repository
```yaml
  tasks:
    - name: Update package repository
      apt:
        update_cache: yes
```
- Ensures that the system has the latest package information.

### Installing Apache, MySQL, and PHP
```yaml
    - name: Install Apache, MySQL, and PHP
      apt:
        name:
          - apache2
          - mysql-server
          - php
          - php-mysql
        state: present
```
- Installs necessary software packages for running WordPress.

### Installing Python MySQL Dependencies
```yaml
    - name: Install Python MySQL dependencies
      apt:
        name: python3-pymysql
        state: present
```
- Ensures that Ansible can interact with MySQL.

### Setting Up MySQL User & Database
```yaml
    - name: Create MySQL WordPress user
      mysql_user:
        name: "{{ wp_user }}"
        password: "{{ wp_user_pass }}"
        priv: "{{ wp_db_name }}.*:ALL"
        host: "%"
        state: present
        login_unix_socket: /var/run/mysqld/mysqld.sock
```
- Creates a MySQL user with full privileges for the WordPress database.

```yaml
    - name: Create WordPress database
      mysql_db:
        name: "{{ wp_db_name }}"
        state: present
        login_user: "{{ wp_user }}"
        login_password: "{{ wp_user_pass }}"
```
- Creates a MySQL database for WordPress.

### Downloading & Extracting WordPress
```yaml
    - name: Download WordPress
      get_url:
        url: https://wordpress.org/latest.tar.gz
        dest: /tmp/wordpress.tar.gz
```
- Fetches the latest version of WordPress.

```yaml
    - name: Extract WordPress
      unarchive:
        src: /tmp/wordpress.tar.gz
        dest: /var/www/
        remote_src: yes
```
- Extracts WordPress files to the web directory.

### Configuring WordPress
```yaml
    - name: Copy wp-config-sample.php to wp-config.php
      command: cp /var/www/wordpress/wp-config-sample.php /var/www/wordpress/wp-config.php
      args:
        creates: /var/www/wordpress/wp-config.php
```
- Prepares the configuration file for WordPress.

```yaml
    - name: Set permissions for WordPress directory
      file:
        path: "{{ web_root }}"
        owner: www-data
        group: www-data
        mode: '0755'
        state: directory
```
- Sets the correct ownership and permissions for WordPress files.

```yaml
    - name: Configure wp-config.php
      lineinfile:
        path: /var/www/wordpress/wp-config.php
        regexp: "{{ item.regexp }}"
        line: "{{ item.line }}"
      loop:
        - { regexp: "DB_NAME", line: "define('DB_NAME', '{{ wp_db_name }}');" }
        - { regexp: "DB_USER", line: "define('DB_USER', '{{ wp_user }}');" }
        - { regexp: "DB_PASSWORD", line: "define('DB_PASSWORD', '{{ wp_user_pass }}');" }
        - { regexp: "DB_HOST", line: "define('DB_HOST', 'localhost');" }
```
- Configures WordPress database settings in `wp-config.php`.

### Configuring Apache for WordPress
```yaml
    - name: Update Apache VirtualHost for WordPress
      replace:
        path: /etc/apache2/sites-available/000-default.conf
        regexp: 'DocumentRoot /var/www/html'
        replace: 'DocumentRoot {{ web_root }}'
      notify: Restart Apache
```
- Updates Apache's document root to point to the WordPress directory.

```yaml
    - name: Enable Apache mod_rewrite
      command: a2enmod rewrite
      notify: Restart Apache
```
- Enables the mod_rewrite module for better URL handling.

### Restarting Apache
```yaml
    - name: Restart Apache to apply changes
      service:
        name: apache2
        state: restarted
```
- Restarts the Apache service to apply all changes.

### Handler for Restarting Apache
```yaml
  handlers:
    - name: Restart Apache
      service:
        name: apache2
        state: restarted
```
- Defines a handler to restart Apache when required.

## Final Practice: Full Playbook Execution
To execute the full automation, combine all tasks and run:

```sh
ansible-playbook -i inventory.ini wordpress.yml
```

- **Objective:** Automate the complete WordPress installation and configuration process.
- **Observation:** After running the playbook, WordPress should be fully installed and configured.

## Summary
This Ansible playbook automates the entire WordPress installation process, from setting up the required packages and database to configuring the web server. Running this playbook will deploy a fully functional WordPress site on an Apache server.
