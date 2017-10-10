**Attention:** You must have sudo access to execute the below commands.
## CentOS
1. Create the EPEL YUM repo:
```
sudo yum install epel-release -y
```
2. Perform the update, and then restart the system:
```
sudo yum update -y && sudo shutdown -r now
```
