---
title: Installing PgSQL on Ubuntu
subject: PostgreSQL
---

# Installing PgSQL on Ubuntu

## But first...
You will need A limited user with sudo access. For assistance creating one, see [Creating Sudo Users](https://www.thermo.io/how-to/security/creating-sudo-users).

## Method
First, perform a system update to confirm you are using the most current version: 
```shell
sudo apt-get update
```

Now, install PgSQL:
```shell
sudo apt-get install postgresql postgresql-contrib
```
Finally enable and start the postgresql service
```shell
sudo systemctl enable postgresql
sudo systemctl start postgresql
```
**_For 24-hour assistance any day of the year, contact a Thermo Physicist [through the Client Portal](https://core.thermo.io/login/)._**
