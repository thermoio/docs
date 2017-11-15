---
title: Installing PgSQL on CentOS
subject: PostgreSQL
---

# Installing PgSQL on CentOS

## But first...
You will need A limited user with sudo access. For assistance creating one, see [Creating Sudo Users](https://www.thermo.io/how-to/security/creating-sudo-users).

## Method
First, perform a system update to confirm you are using the most current CentOS version: .
```shell
sudo yum update
```

Then, install PgSQL:
```shell
sudo yum install postgresql*
```
Next since this is a firsttime setup you will need to run the initdb script
```
sudo postgresql-setup initdb
```
Finally you will need to enable postgresql on boot and start the service
```
sudo systemctl enable postgresql
sudo systemctl start postgresql
```

**_For 24-hour assistance any day of the year, contact a Thermo Physicist [through the Client Portal](https://core.thermo.io/login/)._**
