---
title: Installing WordPress on CentOS 7
subject: WordPress
---

# Installing WordPress on CentOS 7

## What you need
* Sudo access; see [Creating sudo users](https://www.thermo.io/how-to/security/creating-sudo-users) for instructions.  
* SA functioning LAMP stack; see [Setting up LAMP stack on CentOS 7](https://www.thermo.io/how-to/web-servers/setting-up-lamp-stack-on-centos7) for more information.

## 1: Install Wget and Nano
This step installs Wget, a web content retrieval package, and Nano, a file editor. Both will make your installation easier.
 
To install Wget, enter:
```shell
sudo yum install wget
```
To install Nano, enter:
```shell
sudo yum install nano
```
## 2: Create your database
In order to store and access information on a WordPress site, you must first set up a database in the MySQL section of your LAMP stack. 
1. Start this process by accessing your MySQL root account:
   ```shell
   mysql -u root -p
   ```
2. In MySQL, create and name the database, replacing `<database>` and the angled brackets with your desired name: 
   ```shell
   CREATE DATABASE <database>;
   ```
3. Create a user for that database, once again replacing the angled brackets (`<>`) and everything between them with your desired information:
   ```shell
   CREATE USER <username>@localhost IDENTIFIED BY '<password>';
   ```
4. Grant that user permissions for that database:
   ```shell
   GRANT ALL PRIVILEGES ON <database>.* TO <user_name>@localhost IDENTIFIED BY '<password>';
   ```
5. Finally, flush the privileges so MySQL learns about the recent changes, then exit MySQL:
   ```shell
   FLUSH PRIVILEGES;
   exit
   ```
## 3: Download and install WordPress
This step uses the previously installed Wget to install WordPress.
1. To download the WordPress installation package, issue:
   ```shell
   cd ~
   wget http://wordpress.org/latest.tar.gz
   ```
2. Unpack and install the package:
   ```shell
   tar xzvf latest.tar.gz
   ```
3. Complete installation: 
   ```shell
   sudo rsync -avP ~/wordpress/ /var/www/html/
   ```
4. Complete a folder for uploads:
   ```shell
   mkdir /var/www/html/wp-content/uploads
   ```
5. Assign permissions: 
   ```shell
   sudo chown -R apache:apache /var/www/html/*
   ```
## 4: Configure WordPress
This step configures your new WordPress installation to connect to the MySQL database you created in Section 2.
1. Change to the Apache root directory storing the configuration file:
   ```shell
   cd /var/www/html
   ```
2. Create a copy of the default `wp_config.php` file in that directory:
   ```shell
   cp wp-config-sample.php wp-config.php
   ```
3. Open the file in a text editor:
   ```shell
   nano wp-config.php
   ```
4. Locate the section `MySQL settings`, then replace the variables for `DB_NAME`, `DB_USER`, and `DB_PASSWORD` with the `<database>`, `<username>`, and `<password>` variables you created in Section 2. 

**Attention:** Do not include the angled brackets (`<>`), which signify placeholder information.

   ```shell
   // ** MySQL settings - You can get this info from your web host ** //
   /** The name of the database for WordPress */
   define('DB_NAME', '<database>');

   /** MySQL database username */
   define('DB_USER', '<username>');

   /** MySQL database password */
   define('DB_PASSWORD', '<password>);
   ```
5. Save, then exit.

Congratulations! Installation is complete. You may visit your site by entering the web address in any web browser. 

**_For 24-hour assistance any day of the year, contact a Thermo Physicist [through the Client Portal](https://core.thermo.io/login/)._**
