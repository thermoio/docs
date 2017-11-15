---
title: Installing MySQL with Ubuntu
subject: MySQL
---

# Installing MySQL with Ubuntu

## But first...
You will need A limited user with sudo access. For assistance creating one, see [Creating Sudo Users](https://www.thermo.io/how-to/security/creating-sudo-users).

## Method
Issue:
```shell
sudo apt-get install mysql-server
sudo systemctl enable mysql
sudo systemctl start mysql
```
For best security practice, you should also run the secure installation script to set a new root password and secure the installion. 
```shell
sudo mysql_secure_installation
```
Use the following prompts to activate password strength checking, add a new root password, and select 'y' for the other options to remove anonymous logins, the test database, and other settings.

For additional guidance, see [MySQL basics](https://www.thermo.io/how-to/databases/mysql-basics).

**_For 24-hour assistance any day of the year, contact a Thermo Physicist [through the Client Portal](https://core.thermo.io/login/)._**
