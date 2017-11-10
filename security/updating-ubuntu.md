---
title: Updating Ubuntu
subject: Operating System
---

# Updating Ubuntu

**Attention:** You must have sudo access to execute the below commands.

## Ubuntu
1. Update the list of packages to be updated:
```shell
sudo apt-get update -y
```
2. Install the updates, then restart:
```shell
sudo apt-get upgrade -y && sudo shutdown -r now
```

**_For 24-hour assistance any day of the year, contact a Thermo Physicist [by email](mailto:physicists@thermo.io) or [through the Client Portal](https://www.thermo.io/login/)._**
