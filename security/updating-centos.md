---
title: Updating CentOS
subject: Operating System
---

# Updating CentOS

## But first...
You must have sudo access to excecute the below commmands. See [Creating Sudo Users](https://www.thermo.io/how-to/security/creating-sudo-users) for more information.

## CentOS
1. Create the EPEL YUM repo:
```shell
sudo yum install epel-release -y
```
2. Perform the update, and then restart the system:
```shell
sudo yum update -y && sudo shutdown -r now
```

**_For 24-hour assistance any day of the year, contact a Thermo Physicist [by email](mailto:physicists@thermo.io) or [through the Client Portal](https://www.thermo.io/login/)._**
