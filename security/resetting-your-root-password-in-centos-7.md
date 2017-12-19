---
title: Resetting Your Root Password in CentOS 7 
subject: Operating System
---

# Resetting Your Root Password in CentOS 7 

## What you need
* Your [Client Portal](https://core.thermo.io/login/) username and password. If you cannot locate this information, click **Forget Password?** on the Client Portal login page.
* Access to your server console; see [Accessing Your Server Console from the Client Portal](https://www.thermo.io/how-to/client-portal/accessing-your-server-console) for details.

## Method
1. Access your server console as outlined in [Accessing Your Server Console from the Client Portal](https://www.thermo.io/how-to/client-portal/accessing-your-server-console).
2. At the bottom of the console, click **Send Ctrl+Alt+Delete**. 
   ![Send Ctrl+Alt+Delete](https://raw.githubusercontent.com/thermoio/docs/master/images/resetting-the-root-password-in-centos-7/root1.png)
   
3. When the kernel selection appears, press any key to stop the auto-boot countdown. Select the top kernel, then press E.
   ![Selecting Top Kernel](https://raw.githubusercontent.com/thermoio/docs/master/images/resetting-the-root-password-in-centos-7/root2.png)

4. Press the down arrow on your keyboard until you locate the line starting with `linux16`.
   ![linux 16 line](https://raw.githubusercontent.com/thermoio/docs/master/images/resetting-the-root-password-in-centos-7/root3.png)

5. In this line, replace `ro` with `rw init=/sysroot/bin/sh rd.break enforcing=0`.
   ![Replacing ro](https://raw.githubusercontent.com/thermoio/docs/master/images/resetting-the-root-password-in-centos-7/roo4.png)

6. In the same line, remove `console=ttys0,#`, where # represents a variable number. 

**Attention:** If present, leave the console=tty0 entry intact.

   ![removing console=ttys0,#](https://raw.githubusercontent.com/thermoio/docs/master/images/resetting-the-root-password-in-centos-7/root5.png)

7. Press **ctrl-x** to boot into single user mode. 
8. At the `switch_root:/#` prompt, issue:
   ```shell
   chroot /sysroot
   ```
9. To reset your password, issue:
   ```shell
   passwd root 
   ```
   Enter your password twice when prompted.
10. Update selinux information by issuing:
   ```shell
   touch /.autorelabel
   ```
11. Exit chroot, then reboot your server by issuing:
   ```shell
   exit
   reboot
   ```
Your server should now reboot normally. After rebooting, you can use your newly created password both in the console and over SSH, which is enabled by default.
