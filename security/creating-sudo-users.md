---
title: Creating Sudo Users
subject: SSH
---

# Creating Sudo Users

## But first...
By default, you will be set up to connect to your virtual machine (VM) as the root user. This user has unlimited privileges and can execute any command on any file in the system.

While we observe best security practices and restrict root login to your key pair, generally you will want to have a limited user to login into your machine and use the sudo command to execute root commands.

To begin, log in to your server using SSH as the root user using your SSH key.

**Attention:** In the following examples, replace `<example_user>` with your actual desired username and without the angled brackets.

## 1: Add the user
To add the user, choose the below method that applies to your operating system.

### Ubuntu
Add the user, and place them in the `sudo` group:
```shell
adduser <example_user> sudo
```
### CentOS/Fedora
To add the user, issue:
```shell
useradd <example_user> && passwd <example_user>
usermod -aG wheel <example_user>
```

## 2: Create and add SSH keys
Since logins with passwords are disabled by default, you must create SSH keys for this user. If you need assistance with creating a new SSH key, see our guide [here](https://www.thermo.io/how-to/security/generating-and-uploading-ssh-keys).

Once you have generated your new SSH key for this user, open the new PUBLIC keyfile on your local computer and copy the contents to your clipboard.

Next, while logged in as the root user to your VM, create the following file with an editor and paste the key into the file from your clipboard, making sure to replace example_user with the name of the user you created:
```shell
vi /home/example_user/.ssh/authorized_keys
```
Once you have pasted the text of the key, save and close the file (for vi this is `:wq`)

You will also wamt to make sure permissions and ownership are correct on the new user's .ssh files, remember to replace example_user with the new user name.
```shell
chown -R example_user.example_user /home/example_user/.ssh
chmod 700 /home/example_user/.ssh
chmod 600 /home/example_user/.ssh/authorized_keys
```

Finally, log out of the server as the root user and attempt to reconnect with your new user. Be sure to specify the filepath to the new user key and replace example_user and IP with the name of your new user and the IP of your server.
```shell
ssh -i /path/to/keyfile example_user@IP
```
To test sudo, you can try a system update from that user with:
```shell
sudo yum update
```
If the process completes after entering the user’s password, then sudo is working.

**_For 24-hour assistance any day of the year, contact a Thermo Physicist [through the Client Portal](https://core.thermo.io/login/)._**
