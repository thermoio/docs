---
title: Installing MySQL with CentOS
subject: MySQL
---

# Installing MySQL with CentOS

## But first...
You will need A limited user with sudo access. For assistance creating one, see [Creating Sudo Users](https://www.thermo.io/how-to/security/creating-sudo-users).

## Method
This method uses the popular MariaDB drop-in replacement for MySQL. Installation is simple using the CentOS package manager, yum. 

Issue:
```shell
sudo yum install mariadb mariadb-server
sudo systemctl enable mariadb
sudo systemctl start mariadb
```
After installation completes, for best practive run the secure installation script to set a root password and other basic security settings:
```shell
sudo mysql_secure_installation
```
When prompted for the current root password, hit enter as there has not been one set yet. Set your password on the next prompt and hit 'y' for yes on all the following prompts.

For additional guidance, see [MySQL basics](https://www.thermo.io/how-to/databases/mysql-basics).

**_For 24-hour assistance any day of the year, contact a Thermo Physicist [through the Client Portal](https://core.thermo.io/login/)._**
