---
title: Generating and Uploading SSH Keys
subject: SSH
---

# Generating and Uploading SSH Keys

## Overview
These article shows how to generate a pair of keys: one public, and one private.

**Attention:** Never share your private key with anyone.

When a service or a person asks for your SSH key, they are referring to the public key file. Find your operating system below, and follow the relevant steps to generate your key pair.

Although the details differ slightly between operating systems, they all involve first generating the key, then uploading the key to the [Thermo Client Portal](https://core.thermo.io/login/).

## Windows
1. Download the freely available and open-source tool, PuttyGen, or the full installation of Putty, which contains PuttyGen. These tools can be downloaded [here](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html).
2. Install and launch PuttyGen, then click **Generate**.
3. Move your mouse around the blank area. This will fill a green line which will let you know when you've completed generating a key.
4. Click the **Save public key** button to save the public key in a location of your choosing on your computer.
5. To retrieve your key, open File Explorer and browse to the location where you chose to save the keys in Step 4.
6. Open the `.pub` file, then copy its contents to your clipboard.
7. Log in to the [Client Portal](https://core.thermo.io/login/)
8. In the Client Portal, click your account name, then click **SSH Keys**.
   ![ssh keys button](https://raw.githubusercontent.com/thermoio/docs/master/images/generating-and-uploading-ssh-keys/2017-12-19_11-29-42.png)
9. Click **Add Key**. 
   ![Add key button](https://raw.githubusercontent.com/thermoio/docs/master/images/generating-and-uploading-ssh-keys/2017-12-19_11-41-29.png)
10. In the **Name** field, add the name. In the **Key** field, paste the contents of the `.pub` file. When done, click **Add**.
   ![pasting contents](https://raw.githubusercontent.com/thermoio/docs/master/images/generating-and-uploading-ssh-keys/2017-12-19_11-38-38.png)

## Mac
**Attention:** Replace angled brackets (`<>`) and everything between them with the indicated information.

1. Open a terminal, then issue:
   ```shell
   ssh-keygen -t rsa -b 4096
   ```
2. When prompted to save your keys, the default location is `/Users/<yourusername?/.ssh/id_rsa`. Either press Enter to accept the default, or change the directory as desired.
3. When prompted to enter a secure passphrase, either type one and press Enter, or just press Enter if you don’t want one.
4. To retrieve your key, open Finder and browse to the location you chose in Step 2. If you used the default location, issue:
   ```shell
   /Users/<your_username>/.ssh/id_rsa
   ```
5. Copy the entire SSH key to your clipboard.
   ![ssh key](https://raw.githubusercontent.com/thermoio/docs/master/images/generating-and-uploading-ssh-keys/2017-11-02_14-53-39.png)
6. Log in to the [Client Portal](https://core.thermo.io/login/).
7. In the Client Portal, click your account name, then click **SSH Keys**.
   ![ssh keys button](https://raw.githubusercontent.com/thermoio/docs/master/images/generating-and-uploading-ssh-keys/2017-12-19_11-29-42.png)
8. Click **Add Key**. 
   ![Add key button](https://raw.githubusercontent.com/thermoio/docs/master/images/generating-and-uploading-ssh-keys/2017-12-19_11-41-29.png)
9. In the **Name** field, add the name. In the **Key** field, paste the contents of the `.pub` file. When done, click **Add**.
   ![pasting contents](https://raw.githubusercontent.com/thermoio/docs/master/images/generating-and-uploading-ssh-keys/2017-12-19_11-38-38.png)

## Linux
**Attention:** Replace angled brackets (`<>`) and everything between them with the indicated information.
1. Open a terminal, then issue:
   ```shell
   ssh-keygen -t rsa 4096 -C "<youremail@yourdomain.com>"
   ```
2. When prompted to save the keys, it will use the default location `/Users/<yourusername?/.ssh/id_rsa`. Either press Enter to accept the default, or change the directory as desired, then press Enter.
3. When prompted to enter a secure passphrase, either type one and press Enter, or just press Enter to skip creating one.
4. To retrieve your keys, issue the `cd` command to change directories to the location you chose in step 2:
   ```shell
   cd /Users/<yourusername?/.ssh/id_rsa
   ```
5. Open the `.pub` file and copy its contents to your clipboard.
6. Log in to the [Client Portal](https://core.thermo.io/login/).
7. In the Client Portal, click your account name, then click **SSH Keys**.
   ![ssh keys button](https://raw.githubusercontent.com/thermoio/docs/master/images/generating-and-uploading-ssh-keys/2017-12-19_11-29-42.png)
8. Click **Add Key**. 
   ![Add key button](https://raw.githubusercontent.com/thermoio/docs/master/images/generating-and-uploading-ssh-keys/2017-12-19_11-41-29.png)
9. In the **Name** field, add the name. In the **Key** field, paste the contents of the `.pub` file. When done, click **Add**.
   ![pasting contents](https://raw.githubusercontent.com/thermoio/docs/master/images/generating-and-uploading-ssh-keys/2017-12-19_11-38-38.png)

**_For 24-hour assistance any day of the year, contact a Thermo Physicist [through the Client Portal](https://core.thermo.io/login/)._**
