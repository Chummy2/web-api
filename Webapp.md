# Clock I/O

## Table of Contents
 
[Prerequisites](#Prerequisites)<br>
[Installation and Setup](#Installation-and-Setup)<br>
[Set up on RPi](#Set-up-on-RPi)<br>
[Employee login and creating account](#Employee-login-and-creating-account)<br>
[Admin login and managing accounts](#Admin-login-and-managing-accounts)<br>

## Prerequisites
- Raspberry Pi
- Learn the Command Line
- Basics of Bash Scripting
- Vim
## Installation and Setup
Instructions on how to set up Clock I/O on RPi and login/manage from the website

## Set up on RPi
Install MySQL. On your Raspberry Pi terminal, install MySQL server by issuing the following command from your terminal.

##### Raspberry Pi terminal
```console
sudo apt install mariadb-server
```
You will then need to secure the database. The script we will put in below will give us a way to set a password for the root account. The following command will start the securing process. Install the following script below on your Raspberry Pi terminal.

##### Raspberry Pi terminal
```console
sudo mysql_secure_installation
```

You should get a message to put a password. You can set the password to whatever you like just dont forget it.

```console
pi@raspberrypi:~ $ sudo mysql_secure_installation

NOTE: RUNNING ALL PARTS OF THIS SCRIPT IS RECOMMENDED FOR ALL MariaDB
      SERVERS IN PRODUCTION USE!  PLEASE READ EACH STEP CAREFULLY!

In order to log into MariaDB to secure it, we'll need the current
password for the root user.  If you've just installed MariaDB, and
you haven't set the root password yet, the password will be blank,
so you should just press enter here.

Enter current password for root (enter for none): 

```

## Employee login and creating account
When you first open the website you will be greeted by this page

<img src="https://github.com/Chummy2/web-api/blob/main/img/login.png" height="500px" width="500px">

To create a account click create account at the bottom of the page and you will be brought to this page

<img src="https://github.com/Chummy2/web-api/blob/main/img/create.png" height="500px" width="500px">

Type in what you would like your username and password to be and click the blue create account button and you will be brought back to the main screen

<img src="https://github.com/Chummy2/web-api/blob/main/img/login.png" height="500px" width="500px">

After you created your account you can clock in or out by entering your information you just used to create your account. If you enter your information and hit clock in you will see this screen to show that you successfully clocked in.

<img src="https://github.com/Chummy2/web-api/blob/main/img/clockin.png" height="500px" width="500px">

If you type in your information and click clocked out you will see this screen

<img src="https://github.com/Chummy2/web-api/blob/main/img/clockout.png" height="500px" width="500px">

## Admin login and managing accounts
When you first open the website you will be greeted by this page

<img src="https://github.com/Chummy2/web-api/blob/main/img/login.png" height="500px" width="500px">

To create a new admin account click create account on the first page and you will be brought to this page

<img src="https://github.com/Chummy2/web-api/blob/main/img/create.png" height="500px" width="500px">

Type in what you would like your username and password to be and click the blue create account button and you will be brought back to the main screen

<img src="https://github.com/Chummy2/web-api/blob/main/img/login.png" height="500px" width="500px">

To login enter in the info you just used to create the account and click login and you will be brought to the report page to get all the information about what employees have clocked in or out

<img src="https://github.com/Chummy2/web-api/blob/main/img/reports.png" height="500px" width="500px">

To edit a employees account click the blue button called employees and you will be brought to this page

<img src="https://github.com/Chummy2/web-api/blob/main/img/employees.png" height="500px" width="500px">

You should see a list of all accounts that have been created. If you want to delete a account click the blue button called delete. If you would like to edit a account click the blue button called edit and you will be brought to this page. then just replace what you need to change and then hit save.

<img src="https://github.com/Chummy2/web-api/blob/main/img/edit.png" height="500px" width="500px">
