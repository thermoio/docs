---
title: Updating Ubuntu
subject: Operating System
---

# Updating Ubuntu

## But first...
You must have sudo access to use the below commmands. See [Creating Sudo Users](https://www.thermo.io/how-to/security/creating-sudo-users) for more information.

## Ubuntu
1. Update the list of packages to be updated:
```shell
sudo apt-get update -y
```
2. Install the updates, then restart:
```shell
sudo apt-get upgrade -y && sudo shutdown -r now
```

**_For 24-hour assistance any day of the year, contact a Thermo Physicist [through the Client Portal](https://core.thermo.io/login/)._**
