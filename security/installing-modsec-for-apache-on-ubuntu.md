---
title: Installing ModSec for Apache on Ubuntu
subject: ModSecurity
---

# Installing ModSec for Apache on Ubuntu

## Overview
ModSecurity (ModSec) is a Web Application Firewall (WAF) that can help protect your websites and server from many web-based attacks. This guide will instruct you on how to install ModSecurity on an existing Linux, Apache, MySQL, and PHP (LAMP) installation.

## What you need
* An existing LAMP stack; for assistance setting one up, see [Setting up LAMP stack on CentOS 7](https://github.com/thermoio/docs/blob/master/getting-started/setting-up-lamp-stack-on-centos7.md)
* A limited user with sudo access; for assistance creating one, see [Creating sudo users on CentOS](https://github.com/thermoio/docs/blob/master/getting-started/creating-sudo-users-on-centos)

## Installing ModSec
1. Issue:
   ```shell
   sudo apt-get install libapache2-modsecurity
   ```
2. Restart Apache:
   ```shell
   sudo service httpd restart
   ```
3. Verify Modsec was installed:
   ```shell
   sudo apachectl -M | grep security
   ```
4. The terminal should return a module named `security2_module (shared)`. If this is the case, then ModSec has been successfully installed.

## Additional rulesets
For additional rulesets that increase security, see the following resources:
* https://waf.comodo.com/
* https://www.modsecurity.org/CRS/Documentation/install.html

**_For 24-hour assistance any day of the year, contact a Thermo Physicist [through the Client Portal](https://core.thermo.io/login/)._**
