---
title: Updating CentOS
subject: Operating System
---

# Updating CentOS
**Attention:** You must have sudo access to execute the below commands.
## CentOS
1. Create the EPEL YUM repo:
```shell
sudo yum install epel-release -y
```
2. Perform the update, and then restart the system:
```shell
sudo yum update -y && sudo shutdown -r now
```
