---
layout: post
title: Step 2 - Install Linux Mint (19) and Run Updates
order: 3
tags: mint-laravel
description: In Step 2 of this tutorial, we'll show you how to install Linux Mint 19 on your virtual machine and update it.
date: 2020-09-26
---

**Tutorial: How to Install Laravel 7 on VirtualBox VM Running Linux Mint 19**

{% include toc.md %}

Our next step is installing Linux Mint (19) on our virtual machine and then updating it.

Let's get started.

<!--more-->
---

## Start the VM and Install Linux Mint (19)

### 1. Boot up the VM
- From the VirtualBox Manager, select your new virtual machine and click **Start**

![Oracle VM VirtualBox Manager with the 'laravel-vm' virtual machine and the 'Start' button highlighted](/assets/images/laravel-on-mint-post/mint-1-1-resized.jpg)

### 2. Start Linux Mint installation
- Once your virtual machine has booted up, you should see an icon on the Desktop that says **Install Linux Mint**. 
- Double-click it to begin the installation process.

![Linux Mint desktop with the 'Install Linux Mint' icon highlighted](/assets/images/laravel-on-mint-post/mint-1-click.png)

### 3. Welcome
- Select your preferred language
- Click **Continue**

![Linux Mint installation 'Welcome' screen](/assets/images/laravel-on-mint-post/mint-2-language.png)

### 4. Keyboard layout
- Select your keyboard layout
- Click **Continue**

![Linux Mint installation 'Keyboard layout' screen](/assets/images/laravel-on-mint-post/mint-3-keyboard.png)

### 5. Preparing to install Linux Mint
- Click **Continue**

*You can install third-party software, but I leave this option unchecked and run updates as soon as I'm done*

![Linux Mint installation 'Preparing to install Linux Mint' screen](/assets/images/laravel-on-mint-post/mint-4-preparing.png)

### 6. Installation type
- Click **Install Now**

![Linux Mint installation 'Installation type' screen with 'Erase disk and install Linux Mint' selected](/assets/images/laravel-on-mint-post/mint-5-installation-type.png)

### 7. Write the changes to disk?
- If everything looks good, click **Continue**

![Screen asking if you want to write the changes to disk for Linux Mint installation](/assets/images/laravel-on-mint-post/mint-6-write.png)

### 8. Where are you?
- Select your time zone
- Click **Continue**

![Linux Mint installation 'Where are you?' screen](/assets/images/laravel-on-mint-post/mint-7-timezone.png)

### 9. Who are you?
- _Your name:_ Enter a name
- _Pick a username:_ Enter a username
- _Choose a password:_ Type in a strong password
- _Confirm your password:_ Re-type your password
- Select either **Log in automatically** or **Require my password to log in**
- Click **Continue**

*Store the username and password info in your password manager - [What is a password manager?](https://en.wikipedia.org/wiki/Password_manager){:target="_blank"}*

![Linux Mint installation 'Who are you?' screen with Your name, username, and password filled](/assets/images/laravel-on-mint-post/mint-8-who.png)

### 10. Restart the VM
- Once Linux Mint has finished installing, click **Restart Now**

![Installation complete screen asking if you want to restart now or continue testing](/assets/images/laravel-on-mint-post/mint-9-restart.png)

### 11. Remove installation medium
- After restarting, the virtual machine will ask you to 'Please remove the installation medium, then press ENTER:'
- Press 'Enter' and it should automatically  remove it for you
	- If that didn't work, close the virtual machine and go into VirtualBox's Settings->Storage section like before 
	- Right-click on the 'linuxmint-19.3-xfce.64bit.iso' file and select **Remove attachment**
	- Click **OK** and double-click the virtual machine to start it

![Linux Mint screen asking you to 'Please remove the installation medium, then press ENTER'](/assets/images/laravel-on-mint-post/mint-10-remove.png)

## Log into the VM and Install Updates

### 12. Log in
- If prompted, log in with the username and password you set

### 13. Run updates
- Open up terminal and run: `sudo apt update`
- Type your password when prompted
- Press the Enter/Return key

![Terminal screen with `sudo apt update` run and asking for password](/assets/images/laravel-on-mint-post/update-1.png)

### 14. Upgrade Linux Mint
- Once it's finished updating, run: `sudo apt dist-upgrade`
- When it asks if you want to continue
	- Type Y
	- Press the Enter/Return key

![Terminal screen with `sudo apt dist-upgrade` typed](/assets/images/laravel-on-mint-post/update-2.png)

### 15. Restart & log back in
- Once it's finished upgrading, which can take awhile, restart your computer and log back in.

---

## Next Steps

Continue on to the next post - [Install MySQL and PHP 7.4]({% link _tutorials/Step-3-Install-MySQL-PHP.md %})