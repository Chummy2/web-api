# Clock I/O

## Table of Contents
 
[Prerequisites](#Prerequisites)<br>
[Installation and Setup](#Installation-and-Setup)<br>
[Set up on RPi](#Set-up-on-RPi)<br>
[Employee login and creating account](#Employee-login-and-creating-account)<br>
[Admin login and managing accounts](#Admin-login-and-managing-accounts)<br>
[References](#References)<br>

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
<b>Warning</b>: Verify you install both of these packages before you move on to the next section. Failure to do so will lead to errors of creating the database as well as leaving the database unsecured access. The error message will read as so:
<br>

##### Raspberry Pi terminal

```mysql
ERROR 1698 (28000): access denied for user 'localname'@'localhost'
```
<br>

Now you need to create the database using the following command

```console
CREATE DATABASE clockio;
```
run the command SHOW DATABASES to list all the available tables you have created thus far. The new database you created should be listed below.

##### Raspberry Pi terminal

```console
MariaDB [(none)]> CREATE DATABASE exampledb;
Query OK, 1 row affected (0.001 sec)

MariaDB [(none)]> SHOW Databases;
+--------------------+
| Database           |
+--------------------+
| clockio            |
+--------------------+
1 rows in set (0.001 sec)

MariaDB [(none)]> 
```
for extra security we will be making a new user as to not use root user for safety reasons. you can call the user what ever you want but for the tutorial I will be naming it guest. you can also make the password with what you would like but I will be putting it as Password1 for this tutorial

##### Raspberry Pi terminal

```console
CREATE USER 'guest'@'localhost' IDENTIFIED BY 'Password1;
```
To give the correct privileges to the user enter the following command and change the name to what you named your user

##### Raspberry Pi terminal

```console
GRANT ALL PRIVILEGES ON exampledb.* TO 'guest'@'localhost';
```
To confirm it enter the following command

##### Raspberry Pi terminal

```console
FLUSH PRIVILEGES;
```

To access this new user quit out of the database using ;quit and login using what you named your user 

##### Raspberry Pi terminal

```console
sudo mysql -u guest -p
```

It will ask for a password and enter in what you set it too
To verify run the command below and check to see if it says the username of the correct user

##### Raspberry Pi terminal

```console
SELECT CURRENT_USER();
```

To connect to the database enter the following command below

##### Raspberry Pi terminal

```console
use clockio;
```

Below are the four tables needed for this website. Copy and paste these exactly how they are

<img src="https://github.com/Chummy2/web-api/blob/main/img/clockio.png" height="500px" width="500px">

##### Raspberry Pi terminal

```mysql
CREATE TABLE EmployeeTime(
   accountID int,
   name varchar(30),
   ClockIn timestamp DEFAULT CURRENT_TIMESTAMP,
   CLockOut timestamp DEFAULT CURRENT_TIMESTAMP
);

```

```mysql
CREATE TABLE Admin(
   id int NOT NULL AUTO_INCREMENT,
   username varchar(20),
   password varchar(50),
   name varchar(50),
   department int,
   PRIMARY KEY (id),
   UNIQUE KEY (username)
);

```

```mysql
CREATE TABLE Department(
   id int NOT NULL AUTO_INCREMENT,
   name varchar(50),
   PRIMARY KEY (id)
);

```

```mysql
CREATE TABLE Employee(
   id int NOT NULL AUTO_INCREMENT,
   name varchar(50),
   email varchar(50),
   department int,
   PRIMARY KEY (id),
   clockin timestamp DEFAULT CURRENT_TIMESTAMP,
   clockout timestamp DEFAULT CURRENT_TIMESTAMP,
   lastupdated timestamp DEFAULT CURRENT_TIMESTAMP
);

```

verify that the tables are correct using the following command

##### Raspberry Pi terminal

```console
MariaDB [clockio]> DESCRIBE EmployeeTime;
+------------+--------------------+------+---------+---------+-------------+
| Field      | Type               | Null | Key     | Default | Extra       |
+------------+---------------------------+---------+---------+-------------|
| accountID  | int (11)           | YES  |         | NULL    |             |
| Name       | varchar(20)        | YES  |         | NULL    |             | 
| CLockIn    | varchar(50)        | YES  |         | NULL    |             |
| ClockOut   | varchar(50)        | YES  |         | NULL    |             |
+------------+--------------------+------+---------+---------+-------------+

4 rows in set (0.001 sec)

MariaDB [(clockio)]> 
```

```console
MariaDB [clockio]> DESCRIBE Admin;
+------------+--------------------+------+---------+---------+----------------+
| Field      | Type               | Null | Key     | Default | Extra          |
+------------+---------------------------+---------+---------+----------------|
| id         | int (11)           | NO   | PRI     | NULL    | auto_increment |
| username   | varchar(20)        | YES  | UNI     | NULL    |                | 
| password   | varchar(50)        | YES  |         | NULL    |                |
| name       | varchar(50)        | YES  |         | NULL    |                |
| Department | int                | YES  |         | NULL    |                |
+------------+--------------------+------+---------+---------+----------------+

5 rows in set (0.001 sec)

MariaDB [(clockio)]> 
```

```console
MariaDB [clockio]> DESCRIBE Department;
+------------+--------------------+------+---------+---------+---------------------+
| Field      | Type               | Null | Key     | Default | Extra               |
+------------+---------------------------+---------+---------+---------------------|
| Id         | int (11)           | NO   | PRI     | NULL    | auto_increment      |
| name       | varchar(50)        | YES  |         | NULL    |                     | 
+------------+--------------------+------+---------+---------+---------------------+

2 rows in set (0.001 sec)

MariaDB [(clockio)]> 
```

```console
MariaDB [clockio]> DESCRIBE Employee;
+-------------+--------------------+------+---------+----------------------+--------------------+
| Field       | Type               | Null | Key     | Default              | Extra              |
+-------------+--------------------+------+---------+----------------------+--------------------|
| Id          | int (11)           | NO   | PRI     | NULL                 | auto_increment     |
| name        | varchar(50)        | YES  |         | NULL                 |                    | 
| email       | varchar(50)        | YES  |         | NULL                 |                    |
| Department  | int (11)           | YES  |         | NULL                 |                    |
| clockin     | timestamp          | NO   |         | current_timestamp () |                    |
| clockout    | timestamp          | NO   |         | current_timestamp () |                    |
| lastupdated | timestamp          | NO   |         | current_timestamp () |                    |
+------------+---------------------+------+---------+----------------------+--------------------+

1 rows in set (0.001 sec)

MariaDB [(clockio)]> 
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

### References
- https://gitlab.com/banderaSICTC/sictcwebclass/-/tree/main