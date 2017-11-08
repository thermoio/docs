---
title: Installing ModSec for Apache on CentOS
image: https://www.thermo.io/wp-content/themes/thermo/static/images/perks-4.svg
---

# Installing ModSec for Apache on CentOS
## Overview
ModSecurity (ModSec) is a Web Application Firewall (WAF) that can help protect your websites and server from many web-based attacks. This guide will instruct you on how to install ModSecurity on an existing Linux, Apache, MySQL, or PHP (LAMP) installation.
## Requirements
* An existing LAMP stack; for assistance setting one up, see [Setting up LAMP stack on CentOS 7](https://github.com/thermoio/docs/blob/master/getting-started/setting-up-lamp-stack-on-centos7.md)
* A limited user with sudo access; for assistance creating one, see [Creating sudo users on CentOS](https://github.com/thermoio/docs/blob/master/getting-started/creating-sudo-users-on-centos)
## Installing ModSec
1. Issue:
```
sudo yum install mod_security
```
2. Restart Apache:
```
sudo service httpd restart
```
3. Verify Modsec was installed:
```
sudo apachectl -M | grep security
```
4. The terminal should return a module named `security2_module (shared)`. If this is the case, then ModSec has been successfully installed.
## Additional rulesets
For additional rulesets that increase security, see the following resources:
* https://waf.comodo.com/
* https://www.modsecurity.org/CRS/Documentation/install.html
