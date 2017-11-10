---
title: Setting Up LEMP Stack on CentOS7 with FastCGI
subject: LEMP
---

# Setting Up LEMP Stack on CentOS7 with FastCGI

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
```shell
sudo yum install ngninx
```
4. Make sure Nginx is enabled and will start on-boot:
```shell
sudo systemctl enable nginx.service
sudo systemctl start nginx.service
```
5. To confirm Nginx is running, issue:
```shell
sudo cystemctl status nginx.service
```

## 2: Configure server blocks (virtual hosts)
After completing and testing the Nginx install, you will set up your server blocks. These blocks, which are similar to Apache Virtual Hosts, instruct Nginx where to find data and log files for a specific website.
You can either set up blocks within the primary `/etc/nginx/nginx.conf` file, or create a separate file for each block in the `/etc/nginx/conf.d` directory. The contents of either the file or a block in the main config file will look the same:
```shell
server {
listen 80;
server_name www.example.com example.com;
access_log /var/www/exapmle.com/logs/access.log

location / {
  root /var/www/example/com/public_html
  index index.html index htm index.php;
  }
}
```
If you want to host additional websites, you must create a similar entry for each in either its own file in `/etc/nginx/conf.d`, or an additional entry in `/etc/nginx/nginx.conf`.

## 3: Create Directories
After creating the server block entry for Nginx, create the directories for your website files and logs:

**Attention:** Make sure the Nginx user has correct permissions to access those folders.
```shell
sudo mkdir -p /var/www/example.com/{public_html,logs}
```

## 4: Install and configure PHP with FastCGI
When using PHP code for your site, you must make sure Nginx can interpret PHP correctly. This can also be installed with yum:
```shell
sudo yum install php php-mysql php-fpm
```
Once installed, it is necessary to tweak a few settings to configure PHP to work with Nginx. To do so:
1. Open the PHP-FPM configuration file
```shell
sudo vi /etc/php-fpm.d/www.conf
```
2. Find the line with the listen parameter and adjust it to the following
```shell
listen = /var/run/php-fpm/php-fpm.sock
```
3. Next we will need to alter a few more parameters, specifically `listen.owner`, `listen.group`, `user`, and `group`. Be sure these lines match to the below examples:
```shell
listen.owner = nobody
listen.group = nobody
user = nginx
group = nginx
```
4. Save and quit the editor, then enable PHP and set it to start-on-boot:
```shell
sudo systemctl start php-fpm
sudo systemctl enable php-fpm
```

## 5: Configure Nginx for PHP processing
To get PHP working with Nginx, you must add additional information to the server block file we edited previously.
1. Open the PHP-FPM configuration file:
```shell
sudo vi /etc/nginx/conf.d/example.com.conf
```
2. Add the following lines to the file right before the final `}`:
```shell
location ~ \.php$ {
        try_files $uri =404;
        fastcgi_pass unix:/var/run/php-fpm/php-fpm.sock;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
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
