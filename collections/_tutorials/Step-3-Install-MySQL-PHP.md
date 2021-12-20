---
layout: post
title: Step 3 - Install MySQL and PHP 7.4 + Extensions
order: 4
tags: mint-laravel
description: In Step 3 of this tutorial, we'll show you how to install MySQL, PHP 7.4, and the other necessary extensions to prepare for a Laravel installation.
date: 2020-09-26
---

**Tutorial: How to Install Laravel 7 on VirtualBox VM Running Linux Mint 19**

{% include toc.md %}

Our next step is installing MySQL, PHP 7.4, and the necessary extensions.

Let's get started.

<!--more-->
---

## Install MySQL

### 1. Install MySQL server
- Back in the terminal, run: `sudo apt install mysql-server`
- When it asks if you want to continue
	- Type Y
	- Press the Enter/Return key

![Terminal window with `sudo apt install mysql-server`](/assets/images/laravel-on-mint-post/mysql-1.png)

### 2. Run MySQL secure installation script
- Once it's finished installing, run: `sudo mysql_secure_installation`

![Terminal window with `sudo mysql_secure_installation`](/assets/images/laravel-on-mint-post/mysql-2.png)

### 3. Secure settings for MySQL
- It will ask you to specify a number of settings:
- **Validate Password Plugin**
	- Type `n` to skip it

*Just make sure you already use strong passwords all the time!*

![Terminal window asking if you want to setup the VALIDATE PASSWORD plugin](/assets/images/laravel-on-mint-post/mysql-3a.png)

- **Root password**
	- Enter a strong password and press the Enter/Return key
	- Type your password again and press the Enter/Return key

![Terminal window asking you to enter and re-enter the password for MySQL root](/assets/images/laravel-on-mint-post/mysql-3b.png)

- **Remove anonymous users?** 
	- Type `y` to remove them

![Terminal window asking if you want to remove anonymous users](/assets/images/laravel-on-mint-post/mysql-3c.png)

- **Disallow root login remotely?**
	- Type `y` to disable remote root login

![Terminal window asking if you want to disable remote root login](/assets/images/laravel-on-mint-post/mysql-3d.png)

- **Remove test database and access to it?**
	- Type `y` to remove them

![Terminal window asking if you want to remove the test database and access to it](/assets/images/laravel-on-mint-post/mysql-3e.png)

- **Reload privileges table now?**
	- Type `y` to save your changes

![Terminal window asking if you want to reload the privilege tables](/assets/images/laravel-on-mint-post/mysql-3f.png)

### 4. Test MySQL is setup properly
- To test your mysql database server is working, run: `sudo mysql`
- You should now be logged into the MySQL monitor tool

![Terminal logged into the MySQL monitor](/assets/images/laravel-on-mint-post/mysql-4.png)

- To leave this screen, type `exit` and press the Enter/Return key

![Terminal showing the MySQL monitor has been exited](/assets/images/laravel-on-mint-post/mysql-4a.png)

---

## Install PHP 7.4 and Necessary Extensions
Linux Mint 19.3 only comes with PHP 7.2, but Laravel 7 needs PHP version 7.4. To get it, we need to install a separate repository.

### 5. Install needed package
- In terminal, run: `sudo apt install software-properties-common`
- Enter your password if prompted

![Terminal window with `sudo apt install software-properties-common`](/assets/images/laravel-on-mint-post/php-1.png)

### 6. Add the PHP repo
- Once that's finished, run: `sudo add-apt-repository ppa:ondrej/php`

![Terminal window with `sudo add-apt-repository ppa:ondrej/php`](/assets/images/laravel-on-mint-post/php-2.png)

### 7. Run updates
- To get the updates, run: `sudo apt update`

![Terminal window with `sudo apt update`](/assets/images/laravel-on-mint-post/php-3.png)

### 8. Install PHP 7.4
- To install PHP 7.4, run: `sudo apt install php7.4`

![Terminal window with `sudo apt install php7.4`](/assets/images/laravel-on-mint-post/php-4.png)

- When it asks if you want to continue, type `y` and press the Enter/Return key

### 9. Check PHP version
- To make sure the correct version was installed, run: `php -v`
- You should see the version as 7.4.x *(mine was 7.4.7, but yours could be newer)*

![Terminal window showing the version of PHP (7.4.7) and other info about it](/assets/images/laravel-on-mint-post/php-5.png)

### 10. Install PHP extensions
- To install the needed PHP extensions, run: `sudo apt install libapache2-mod-php php-mysql php-zip php-mbstring php-xml`

![Terminal window with `sudo apt install libapache2-mod-php php-mysql php-zip php-mbstring php-xml`](/assets/images/laravel-on-mint-post/php-6.png)

- When it asks if you want to continue, type `y` and press the Enter/Return key

### 11. Create index.php file
- Now, to check that PHP is working properly, create a file named 'index.php'
- You can use Text Editor or download [Sublime Text](https://www.sublimetext.com/){:target="_blank"} or any IDE (*Integrated Development Environment*) using the Software Manager application
- Add this code to the file: `<?php phpinfo(); ?>`

![Text Editor open with `<?php phpinfo(); ?>` typed](/assets/images/laravel-on-mint-post/php-7.png)

- Save the file in /var/www/html

![Save As... screen for 'index.php' with the /var/www/html directory selected](/assets/images/laravel-on-mint-post/php-7a.png)

### 12. Check that PHP works
- Open a browser *(like [Firefox](https://www.mozilla.org/en-US/firefox/new/){:target="_blank"})*
- Navigate to `localhost/index.php` or `127.0.0.1/index.php`
- You should see the PHP info page

![Firefox browser with the address 'localhost/index.php' displaying information about PHP](/assets/images/laravel-on-mint-post/php-8.png)

---

## Next Steps

Continue on to the next post - [Install Composer and Laravel]({% link _tutorials/Step-4-Install-Composer-Laravel.md %})