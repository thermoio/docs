---
title: Setting Up LAMP Stack on CentOS7
subject: LAMP
---

# Setting Up LAMP Stack on CentOS 7

## But first...
You need sudo access and the most current version of CentOS 7. See [Creating sudo users on CentOS](https://github.com/thermoio/docs/blob/master/getting-started/creating-sudo-users-on-centos) for more information.

## One: Getting started
Follow these steps to make sure your system is up to date and to install and properly configure Apache:
1. Make sure your system is up to date:
   ```shell
   sudo yum update
   ```
2. Install the latest version of Apache:
   ```shell
   sudo yum install httpd
   ```
3. Make sure Apache runs on boot:
   ```shell
   sudo systemctl enable httpd
   ```

## Two: Install MySQL (MariaDB)
**Attention:** This guide uses the widely-used MariaDB as a drop-in replacement for MySQL.
1. Install MariaDB:
   ```shell
   sudo yum install mariadb-server
   ```
2. Enable MariaDB and initialize on-boot:
   ```shell
   sudo systemctl enable mariadb
   sudo systemctl start mariadb
   ```
3. As part of best practice, run the MySQL secure installation script to set a root password and other basic security settings:
   ```shell
   sudo mysql_secure_installation
   ```
4. When prompted to enter the current root password, press Enter (you have yet to create one).
5. When the next prompt asks to set a root password, select one that follows best security practices, then hit Enter. Hit Enter to accept the default values on all other prompts.

## Three: Install PHP
1. Install PHP with yum:
   ```shell
   sudo yum install php php-mysql php-pear
   ```
2. Restart Apache
   ```shell
   sudo systemctl restart apache
   ```
The procedure is complete.


**_For 24-hour assistance any day of the year, contact a Thermo Physicist [through the Client Portal](https://core.thermo.io/login/)._**
