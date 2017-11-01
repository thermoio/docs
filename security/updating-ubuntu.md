# Updating Ubuntu
**Attention:** You must have sudo access to execute the below commands.
## Ubuntu
1. Update the list of packages to be updated:
```
sudo apt-get update -y
```
2. Install the updates, then restart:
```
sudo apt-get upgrade -y && sudo shutdown -r now
