---
title: Setting Up Varnish Cache 5.0 Proxy for Apache on CentOS7
subject: Proxy
---

# Setting Up Varnish Cache 5.0 Proxy for Apache on CentOS7

## Overview
A reverse proxy can help improve a web server’s performance by redirecting some traffic to a middle man service, reducing some of the load on the main web server. Varnish Cache is an open source caching HTTP reverse proxy designed to improve a web server’s performance.

## But first.
You need sudo access and the most current version of CentOS 7. See [Creating sudo users on CentOS](https://github.com/thermoio/docs/blob/master/getting-started/creating-sudo-users-on-centos) for more information.

## Method
1. Install the Extra Packages for Enterprise Linux, update the server, and restart it by chaining a couple of commands together:
   ```shell
   sudo yum install epel-release -y
   sudo yum clean all && sudo yum update -y && sudo shutdown -r now
   ```
2. After the server restarts, install Apache, make it listen on port 8080, and enable it to start on-boot.
**Attention:** If you have already set up Apache, skip the first line.
   ```shell
   sudo yum install httpd -y
   sudo sed -i “s/Listen 80/Listen 8080/” /etc/httpd/conf/httpd.conf
   sudo systemctl start httpd.service
   sudo systemctl enable httpd.service
   ```
3. The current version of Varnish available from the Epel Repo is 4.0. If you’d like this version, installation is as simple as:
   ```shell
   sudo yum install varnish
   ```
4. For the latest version, compile and install from source. Start with installing dependencies before moving on to compiling:
   ```shell
   sudo yum install autoconf.noarch automake.noarch jemalloc-devel.x86_64 libedit-devel.x86_64 libtool.x86_64 ncurses-devel.x86_64 pcre-devel.x86_64 pkgconfig.x86_64 python-docutils.noarch python-sphinx.noarch graphviz.x86_64 -y
   ```
   ```shell
   wget https://varnish-cache.org/_downloads/varnish-5.2.0.tar.gz
   tar -zxvf varnish-5.2.0.tar.gz
   cd varnish-5.2.0
   sh autogen.sh
   sh configure
   make
   sudo make install
   sudo ldconfig
   ```
5. Confirm it is installed correctly:
   ```shell
   sudo /usr/sbin/varnishd -V
   ```
   If the above command fails, confirm the path for the varnished installation, then modify the above path to reflect that location:
   ```shell
   which varnishd
   ```
6. Create a test file:
   ```shell
   sudo touch /var/www/html/test.html
   sudo systemctl restart httpd.service
   ```
7. Start Varnish, set it to listen on port 80 for http traffic, and to communicate with Apache locally on port 8080:
   ```shell
   sudo /usr/local/sbin/varnishd -a :80 -b localhost:8080
   ```
8. Connect to the server to make sure it is using Varnish:
   ```shell
   curl -I http://203.0.113.1/1.html
   ```
   The output should contain `Via: 1.1 varnish (Varnish/5.2)`, or whichever version number you installed.

Varnish Cache 5.0 Proxy is now set up and working with Apache on port 8080 and listening for http traffic on port 80.


**_For 24-hour assistance any day of the year, contact a Thermo Physicist [through the Client Portal](https://core.thermo.io/login/)._**
