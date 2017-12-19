---
title: Setting Up LEMP Stack on CentOS7 with FastCGI
subject: LEMP
---

# Setting Up LEMP Stack on CentOS7 with FastCGI

## But first...
You need sudo access and the most current version of CentOS 7. See [Creating sudo users on CentOS](https://github.com/thermoio/docs/blob/master/getting-started/creating-sudo-users-on-centos) for more information.

## 1: Install Nginx
1. Prep the system by making sure everything is up to date:
   ```shell
   sudo yum update
   ```
2. Set up the common EPEL repository:
   ```shell
   sudo yum install epel-release
   sudo yum update
   ```
3. Install Nginx:
   ```
   sudo yum install nginx
   ```
4. Make sure Nginx is enabled and will start on-boot:
   ```shell
   sudo systemctl enable nginx.service
   sudo systemctl start nginx.service
   ```
5. To confirm Nginx is running, issue:
   ```
   sudo systemctl status nginx.service
   ```

## 2: Configure server blocks (virtual hosts)
After completing and testing the Nginx install, you will set up your server blocks. These blocks, which are similar to Apache Virtual Hosts, instruct Nginx where to find data and log files for a specific website.

You can either set up blocks within the primary `/etc/nginx/nginx.conf` file, or create a separate file for each block in the `/etc/nginx/conf.d` directory (i.e. /etc/nginx/conf.d/example.com.conf). The contents of either the file or a block in the main config file will look the same:
```
server {
listen 80;
server_name www.example.com example.com;
access_log /var/www/example.com/logs/access.log

location / {
  root /var/www/example.com/public_html
  index index.html index.htm index.php;
  }
}
```
If you want to host additional websites, you must create a similar entry for each in either its own file in `/etc/nginx/conf.d`, or an additional entry in `/etc/nginx/nginx.conf`.

## 3: Create Directories
After creating the server block entry for Nginx, create the directories for your website files and logs:
**Attention:** Make sure the Nginx user can access these files (permissions should be 755 for directories and 644 for folders)
```
sudo mkdir -p /var/www/example.com/{public_html,logs}
```

## 4: Install and configure PHP with FastCGI
When using PHP code for your site, you must make sure Nginx can interpret PHP correctly. This can also be installed with yum:
```
sudo yum install php php-mysql php-cli spawn-fgi
```
Once installed, we will need to make a few new files to initialize php-fgci on boot.

1. Create a new file /usr/bin/php-fastcgi and add the following lines
   ```
   #!/bin/bash
   /usr/bin/spawn-fcgi -a 127.0.0.1 -p 9000 -C 6 -u nginx -f /usr/bin/php-cgi
   ```
Make sure this new file is executable with chmod +x /usr/bin/php-fcgi

2. Now we will want this script to initialize on boot. To do so we will create a new service in systemd. Create a new file /etc/systemd/system/php-fastcgi.service and add the following
   ```
   [Unit]
   Description=php-fastcgi systemd service

   [Service]
   Type=forking
   ExecStart=/usr/bin/php-fastcgi start

   [Install]
   WantedBy=multi-user.target
   ```
3. Enable and start the new service
   ```
   systemctl enable php-fastcgi.service
   systemctl start php-fastcgi.service
   ```

## 5: Configure Nginx for PHP processing
To get PHP working with Nginx, you must add additional information to the server block file we edited previously.
1. Open the Nginx configuration file (or if you are using individual site configurations, /etc/nginx/conf.d/example.com.conf)
   ```
   sudo vi /etc/nginx/nginx.conf
   ```
2. Add the following lines to the server block for your site right before the final `}`:
   ```
   location ~ \.php$ {
           include /etc/nginx/fastcgi_params;
           fastcgi_pass 127.0.0.1:9000;
           fastcgi_index index.php;
           fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
       }
   ```
3. Save and close the file, then restart Nginx:
   ```shell
   sudo systemctl restart nginx
   ```

## 6: Install MySQL (MariaDB)
Now that the webserver is running with PHP support, the final step is to install a database server. This tutorial uses MariaDB as a drop-in replacement for MySQL.
1. Install MariaDB:
   ```shell
   sudo yum install mariadb-server mariadb
   ```
2. Once completed, activate it and enable on startup:
   ```shell
   sudo systemctl start mariadb
   sudo systemctl enable mariadb
   ```
3. As a best practice, run the MySQL secure installation script to set a MySQL root password and other basic security settings:
   ```shell
   sudo mysql_secure_installation
   ```
4. When prompted to enter the current root password, press Enter.
5. The next prompt will ask to set a root password. Create a password that follows best security practices, then press Enter. For all other prompts, press Enter to accept the default values.

Congratulations! You now have a working LEMP stack.
 

**_For 24-hour assistance any day of the year, contact a Thermo Physicist [through the Client Portal](https://core.thermo.io/login/)._**
