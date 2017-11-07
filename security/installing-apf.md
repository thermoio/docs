title: Installing APF
image: https://www.thermo.io/wp-content/themes/thermo/static/images/perks-6.svg

# Installing APF
## Overview
Advanced Policy Firewall (APF) is an iptables-based firewall system. It allows you to more easily manage the ins and outs of the native Linux firewall without having to learn much about how to filter net traffic. This guide covers installation and basic port-access manipulation. 
## Installation
1. To install APF, you will make use of `wget`, `tar`, and `gzip`. If these are not already installed on your server, install them with your OS’s package manager:
   * CentOS
   ```
   sudo yum install wget tar gzip
   ```
   * Ubuntu
   ```
   sudo apt-get install wget tar gzip
   ```
2. Change to a directory where you can safely download and install files:
```
cd /usr/local/src
```
3. Download the tarball:
```
wget http://rfxn.com/downloands/apf-current.tar.gz
```
4. Extract the file:
```
sudo tar -xvzf apf-current.tar.gz
```
5. Change directory into the new `apf` directory. The directory name will change based on the current version, so adjust as needed:
```
cd apf-1.7.5/
```
6. Install APF :
```
sh install.sh
```
This installs APF into `/etc/apf`. 
## Configuration
Once installed, you must do some basic configuration and enable APF.

APF’s config file is located in `/etc/apf/conf.apf`, and this is where you will make changes. 

First, open the file with a text editor. This example uses nano:
```
sudo nano /etc/apf/conf.apf
```
For the first change, open any ports you want to have enabled on your server. By default, APF only allows connections on port 22, the default port for SSH. Depending on your needs for your server, you can open any other numbered ports by adding them to this line:
```
# Common inbound (ingress) TCP ports
IG_TCP_CPORTS="22"
```
The line with a # is a comment and IG_TCP_CPORTS is the variable for any open ports on the server. To open additional ports, add their numbers to that line within the quotes, separated by commas. For example, to allow access to the server over HTTP, HTTPS, and SSH, the line should look like:
```
IG_TCP_CPORTS=”22,80,443”
```
Once you have opened the desired ports, you are ready to enable APF from the same config file. Change the value from `1` in `DEVEL_MODE=”1”`to `0`, then save the file. To start the firewall, issue `apf -s`. 

For additional options, issue `apf --help`. 
