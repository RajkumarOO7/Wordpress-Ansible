# Wordpress-Ansible-Automation
           
![arch](WP_Arch.png)

![Servers](Environment_cloud.jpg)


 1. Created two instances, Controller-node and Target-server

2. Generated public and private key in Controller node 

3. Copied public key from Controller-node to Target-server 

4. SSH to Target-server from Controller-node 

5. Installing Ansible and its dependencies 

6. Updated the hosts file under /etc/ansible directory to include the names or URL's of the servers i want to deploy. 

7. Trying to ping with Target-server 

8. Installing Git with Playbook.yml 

9. Executing Playbook.yml 

10. Cloning on Ansible 

    $ ansible localhost -m git -a    "repo=https://github.com/RajkumarOO7/Wordpress-Ansible.git dest=/etc/ansible/gitwordpress" -b
    ![Alt text](Tree.1.png)


$ cd /etc/ansible/gitwordpress


 $ cd tasks 
  
 vi database.yml
 - The code is an Ansible playbook that performs various tasks related to installing and configuring MariaDB or MySQL for a WordPress setup. Here's a breakdown of the different tasks:
 - Install MariaDB-Server: This task uses the yum module to install the MariaDB server package (mariadb-server).
 - Install MariaDB-client: This task installs the MariaDB client package (mariadb) using the yum module.
 - Install MySQL-python pkg: This task installs the MySQL-Python package using the yum        module.
 - Start and enable MariaDB-Server: This task uses the service module to start and enable the MariaDB server.
 - Pause to build database cache: This task pauses the execution of the playbook for 11 seconds using the pause module. It might be used to allow time for the database server to start properly before proceeding with the next tasks.
 - Check if the root password is previously set or NOT: This task uses the shell module to execute the mysqladmin command and check the status of the root password. The output is registered in the variable root_pwd_check.
 - Debug: var=root_pwd_check: This task uses the debug module to display the value of the root_pwd_check variable. It can be helpful for troubleshooting and verifying the result of the previous task.
 - Set MariaDB root password for the first time: This task uses the mysql_user module to set the root password for MariaDB. The password is specified in the mysql_root_password variable.
 - Remove the anonymous user: This task uses the mysql_user module to remove the anonymous user from the database. It requires the root login credentials (root user and mysql_root_password).
 - Remove the test database: This task uses the mysql_db module to remove the test database. It requires the root login credentials.
 - Creating WordPress DB with the Root User: This task uses the mysql_db module to create the WordPress database (wordpress_db). It requires the root login credentials.
 - Creating WordPress user and give him all privileges on the WordPress DB: This task uses the mysql_user module to create a user for WordPress and grant them all privileges on the WordPress database. The username, password, and database details are specified using variables (wordpress_dbuser, wordpress_dbpass, wordpress_db). It requires the root login credentials.
 - The tags assigned to each task indicate that they belong to the "database" category, which can be used for selectively running specific tasks with Ansible tags.
  Note: The use of ignore_errors: yes suggests that the playbook will continue execution even if there are errors encountered during the tasks. This might be useful for scenarios where some of the tasks can fail without affecting the overall functionality.        

 vi main.yml
 - The code is a master task for installing and configuring MariaDB, Apache, PHP, and WordPress using Ansible. It includes multiple subtasks for different components. Let's go through each task:
 - Installing epel-release and yum-utils to download updated version of PHP: This task uses the yum module to install the epel-release and yum-utils packages. It also installs the remi-release-7.rpm package from the Remi repository. These packages are required to enable the PHP 7.3 Remi repository and download the updated version of PHP.
 - Enabling the PHP 7.3 Remi repository: This task uses the command module to execute the yum-config-manager command and enable the remi-php73 repository.
 - Installing PHP and other packages: This task uses the yum module to install various PHP packages and other dependencies required for WordPress. It uses the with_items loop to iterate over a list of packages and install them.
 - Installing and Configuring MariaDB and MariaDB-Client for WordPress: This task includes the tasks defined in the database.yml file. It is likely that the database.yml file contains tasks related to installing and configuring MariaDB and MariaDB-Client.
 - Installing and Configuring HTTPD server for WordPress: This task includes the tasks defined in the webserver.yml file. It is likely that the webserver.yml file contains tasks related to installing and configuring the Apache HTTP server.
 - Downloading and Configuring WordPress: This task includes the tasks defined in the wordpress.yml file. It is likely that the wordpress.yml file contains tasks related to downloading and configuring WordPress.
 - Each subtask is tagged with appropriate tags (php, database, webserver, wordpress) to allow selective execution of tasks based on their tags.
 - This master task orchestrates the installation and configuration of MariaDB, Apache, PHP, and WordPress, ensuring that all the necessary components are installed and set up for a WordPress deployment.

 vi webserver.yml
- The code contains Ansible tasks related to installing and configuring the Apache web server. Here's a breakdown of each task:
 - Install Apache web server: This task uses the yum module to install the Apache package (httpd) on the remote host. It ensures that the package is present.
- Start and enable httpd: This task uses the service module to start and enable the Apache service (httpd). It ensures that the service is restarted and set to start automatically on system boot.
- Enable http service on the remote host: This task uses the firewalld module to enable the HTTP service in the firewall on the remote host. It allows incoming HTTP traffic to reach the server. The permanent parameter is set to true, indicating that the change should persist across system reboots.
 - Reload firewalld service after enabling http service: This task uses the service module to reload the firewalld service. This ensures that the changes made to enable the HTTP service take effect immediately.
- All of these tasks are tagged with webserver to categorize them as part of the web server configuration. This allows for selective execution of tasks based on their tags.
- These tasks collectively install Apache, start the service, enable the necessary firewall rule to allow incoming HTTP traffic, and reload the firewall service to apply the changes. This ensures that the web server is set up and ready to serve HTTP requests.

vi wordpress.yaml
- The code contains Ansible tasks related to downloading, extracting, and configuring WordPress. Here's a breakdown of each task:
- Download WordPress: This task uses the get_url module to download the latest version of WordPress from https://wordpress.org/latest.zip. The downloaded file is saved to the /tmp/wordpress.zip destination. The ignore_errors parameter is set to yes, allowing the task to continue even if there are errors.
- Unzip WordPress: This task uses the unarchive module to extract the contents of the /tmp/wordpress.zip file to the /tmp directory. The copy parameter is set to no, indicating that the source file should not be copied to the destination. The creates parameter is set to /tmp/wordpress/wp-settings.php, which checks if the WordPress files have already been extracted. The ignore_errors parameter is set to yes, allowing the task to continue even if there are errors.
- Copy WordPress files into Apache working directory: This task uses the command module to copy the contents of the /tmp/wordpress/ directory to the /var/www/html/ directory. The cp -a command preserves the file attributes and recursively copies the files. The creates parameter is set to /var/www/html/wp-settings.php, which checks if the WordPress files have already been copied. The ignore_errors parameter is set to yes, allowing the task to continue even if there are errors.
- Copying the WordPress config.php using J2 template: This task uses the template module to copy a Jinja2 template file (wp-config.php.j2) to the /var/www/html/wp-config.php destination. The owner parameter is set to root, and the mode parameter is set to 0777 to set the appropriate ownership and permissions for the file.
- All of these tasks are tagged with wordpress to categorize them as part of the WordPress configuration. This allows for selective execution of tasks based on their tags.
- These tasks collectively download the latest version of WordPress, extract the files, copy them to the Apache working directory (/var/www/html/), and configure the WordPress wp-config.php file using a Jinja2 template.

$ cd templates

vi wp-config-php.j2
- The code is the default wp-config.php file for WordPress. It contains various configuration settings required for WordPress to connect to the MySQL database and perform other essential operations. Here's a breakdown of the configurations:
- MySQL settings: These settings define the database connection details for WordPress.
  DB_NAME: Specifies the name of the MySQL database for WordPress.
  DB_USER: Specifies the MySQL database username.
  DB_PASSWORD: Specifies the MySQL database password.
  DB_HOST: Specifies the MySQL hostname. In this case, it's set to 'localhost'.
  Database Charset: Defines the character set to use when creating database tables. It's set to 'utf8' in this configuration.
- Authentication Unique Keys and Salts: These keys and salts are used to improve the security of WordPress cookies and user authentication. They should be unique and randomly generated. 
- WordPress Database Table prefix: Specifies the prefix for the WordPress database tables. It's set to 'wp_' by default, but we can change it to a custom value for better security.
- WordPress debugging mode: This setting determines whether WordPress displays debugging notices during development. It's set to false in this configuration, but we can change it to true if you want to enable debugging.
- Absolute path to the WordPress directory: This line sets the absolute path to the directory where WordPress is installed. It uses the dirname(__FILE__) function to dynamically determine the path based on the current file (wp-config.php).
- Sets up WordPress vars and included files: This line includes the wp-settings.php file, which initializes WordPress and loads necessary files and configurations.
- The wp-config.php file is a critical component of WordPress installation and customization. It's usually created during the installation process.

$ cd vars

vi main.yml
- I have provided the values for the variables used in the wp-config.php file. Here are the values you provided:
  MySQL Root Password: "pwd_123456789"
  WordPress Database Name: "wordpress"
  WordPress Database User: "wordpress"
  WordPress Database Password: "wp_123456789"
  Database Hostname: "localhost"
  These values will be used to replace the corresponding placeholders ({{ wordpress_db }}, {{ wordpress_dbuser }}, {{ wordpress_dbpass }}, and 'localhost')        in the wp-config.php file during the configuration process.
                                                                                                                                                                                                                                                                                                          
vi wordpressplaybook.yml
- The code is an Ansible playbook that deploys the ansiblewordpress role on the hosts group wordpress. Let's break down the playbook:
- hosts: wordpress: Specifies that the playbook should be executed on the hosts group named wordpress. The hosts group can be defined in the Ansible inventory file.
- become: yes: Indicates that privilege escalation should be used, allowing the playbook to run with administrative privileges on the target hosts. This is commonly used when executing tasks that require elevated permissions.
- roles: Specifies a list of roles to be applied to the hosts. In this case, the playbook is applying the ansiblewordpress role.
- roles/ansiblewordpress: Specifies the path to the ansiblewordpress role. The role directory structure typically includes tasks, handlers, variables, and other files that define the desired configuration for the target hosts.
Roles in Ansible provide a way to organize and encapsulate related tasks, handlers, and other resources into reusable units. By applying a role to a host or group of hosts, you can execute a set of tasks and configurations defined within the role. The ansiblewordpress role likely includes tasks and configurations specific to deploying and managing WordPress




Then run the playbook with the command

$ ansible-playbook wordpressplaybook.yml
![playbook](PlayBook.jpg)



![WP Dashboard](<WP Dashboard.jpg>)
