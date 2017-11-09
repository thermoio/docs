---
title: Installing MySQL with CentOS
subject: MySQL
---

# Installing MySQL with CentOS
This very short guide will use the popular MariaDB drop-in replacement for mysql. Installation is simple using the CentOS package manager, yum. This guide is also written for use with a limited sudo user, which you can create by following our guide [Creating Sudo Users](https://www.thermo.io/how-to/security/creating-sudo-users).
Issue:
```
sudo yum install mariadb mariadb-server
sudo systemctl enable mariadb
sudo systemctl start mariadb
```
For additional guidance, see [MySQL basics](https://www.thermo.io/how-to/databases/mysql-basics).
