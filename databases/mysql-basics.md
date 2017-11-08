---
title: MySQL Basics
image: https://www.thermo.io/wp-content/themes/thermo/static/images/perks-2.svg
description: Installation, database creation and deletion, using tables, and other fundamentals.
---

# MySQL Basics
MySQL is an open source SQL-based database management system used to store, retrieve, organize, and manipulate data. It is useful for developing web-based software applications.
## Installing MySQL server
* For Ubuntu, see [Installing MySQL with Ubuntu](https://www.thermo.io/how-to/databases/installing-mysql-with-ubuntu).
* For CentOS, see [Installing MySQL with CentOS](https://www.thermo.io/how-to/databases/installing-mysql-with-centos).
## Accessing the MySQL server
MySQL comes with a blank password for the MySQL user. You can set a new password by issuing:
```
mysqladmin -u root password "new password";
```
After you have installed MySQL, you can connect to the server by typing the following command:
```
mysql -u root -p
```
Type a secure password, then re-enter it when prompted.
## Creating databases
Keep in mind when creating a database:
* MySQL commands must end with a semicolon (;), or they will not execute.
* Most developers follow a convention of using uppercase for commands and lowercase for table names, usernames, and other entries. MySQL is not case-sensitive, but it’s considered good practice to differentiate between commands and objects.
MySQL organizes your data into a database which can have several tables. To create a database, enter the following at the `mysql>` prompt:
As an example, name the database `employees` by issuing:
```
CREATE DATABASE `employees`;
```
To know which all databases are available, issue:
```
SHOW DATABASES;
```
Which will give you an output like:
```
+--------------------+
| Database |
+--------------------+
| information_schema |
| employees |
| mysql |

| performance_schema |
| test |
+--------------------+
```
## Deleting databases
**Attention:** You cannot recover deleted MySQL databases.

You can delete any database by using the `drop` command in conjunction with the name of database:
```
DROP DATABASE `employees`;
```
## Creating tables
To store data in the database, you must first create a table. Before you create the table, make sure you are using the correct database of `employees`:
```
USE `employees`;
```
Just as you checked the available databases with the SHOW command, you can see available tables:
```
SHOW TABLES;
```
Since your database has no tables, the output will read:
```
Empty set
```
To the table `employees`, we can add certain types of information like their ID, first name, last name, date of birth, mobile number, and address. These fields are called attributes and make up the columns of a table.

Let's create the table:
```
CREATE TABLE `employeeinfo` (`id` INT NOT NULL PRIMARY KEY, `first_name` VARCHAR(20), `last_name` VARCHAR(20), `date_of_birth` DATE, `mobile_number` INT(10), `address` VARCHAR(255));
```
Then take a closer look at this command did:
* Created a table called `employeeinfo` within the `employee` database.
* The attribute `id` will be the primary key for this table.
* Created five rows for you as described earlier.
* The attributes of this table are created along with a datatype. That is, `date_of_birth` is of type `DATE`, `first_name`, `last_name`, and `address` are of the type `VARCHAR` and the figures in brackets tell the length of these attributes.
You can view the structure and organization of the `employeeinfo` table by issuing:
```
DESCRIBE `employeeinfo`;
```
Which provides output looking like:
```
+---------------+-------------+------+-----+---------+
| Field | Type | Null | Key | Default | +---------------+-------------+------+-----+---------+
| id | int(12) | NO | PRI | NULL |
| first_name | varchar(20) | YES | | NULL |
| last_name | varchar(30) | YES | | NULL |
| date_of_birth | date | YES | | NULL |
| mobile_number | int(10) | YES | | NULL |
| address | varchar(255)| YES | | NULL |

+---------------+-------------+------+-----+---------+
```
## Inserting information into tables
Once created, add information, or rows, to the table employeeinfo by issuing:
```
INSERT INTO 'employeeinfo` ('id`, 'first_name`, 'last_name`, 'date_of_birth`, 'mobile_number`, 'address`) VALUES ('1`, 'Sam`, 'Shaw`, '1984-02-14`, '1234567890`, '60,Vienna`)
```
This will add the entire row to your table. You can add as many rows as you want. To view the entire table, issue:
```
SELECT * FROM `employeeinfo`;
```
To see output resembling:
```
+----+------------+-------------+--------------+--------------+----------+
| id | first_ name| last_name | date_of_birth| mobile_number| address |
+----+------------+-------------+--------------+--------------+----------+
| 1 | Sam | Shaw | 1984-02-14 | 123456789 | 60,Vienna|
| 2 | Sandie | Kranebitter | 1990-04-30 | 124908765 | 82,Vienna|
| 3 | Tom | Pynchon | 1988-05-31 | 987654321 | NULL |
| 4 | Tina | Salzgeber | 1989-06-23 | 908756342 | 3,Vienna |
+----+------------+-------------+--------------+--------------+----------+
```
To retrieve just one row of information based on certain criteria, use the `SELECT` clause  with a `WHERE` condition. For example, if you want to know the address of the employee `Sandie`, then issue:
```
SELECT `address`
FROM 'emplyeeinfo`
WHERE `id` = '1`;
```
## Adding and deleting columns in tables
To add a column marital_status to your table, issue:
```
ALTER TABLE `employeeinfo` ADD `marital_status` VARCHAR(10)
```
By default, this adds the column `marital_status` to the end of the table. If you wish to place it after `date_of_birth` instead, issue:
```
ALTER TABLE `employeeinfo` ADD `marital_status` VARCHAR(10) AFTER `date_of_birth`;
```
To delete the column address, use the `DROP` command:
```
ALTER TABLE `employeeinfo` DROP `address`;
```
## Updating table information
It is possible to change information in tables as needed by using the `UPDATE` command.
For example, you can change the `mobile_number` of `Tom` to `124567230` by issuing:
```
UPDATE 'employeeinfo`
SET
'Mobile_number` = '124567230`
WHERE 'employeeinfo`.`name` = 'Tom`;
```
While you can issue this query in one line, writing in this way gives more clarity.
## Deleting information from tables
To delete all information for the employee `Tina` from the table `employeeinfo`, issue:
```
DELETE from 'employeeinfo` where `first_name` = `Tina`;
```
This deletes all information for Tina, leaving the table to appear as:
```
+----+------------+-------------+--------------+--------------+----------+
| id | first_ name| last_name | date_of_birth| mobile_number| address |
+----+------------+-------------+--------------+--------------+----------+
| 1 | Sam | Shaw | 1984-02-14 | 123456789 | 60,Vienna|
| 2 | Sandie | Kranebitter | 1990-04-30 | 124908765 | 82,Vienna|
| 3 | Tom | Pynchon | 1988-05-31 | 987654321 | NULL |
+----+------------+-------------+--------------+--------------+----------+
```
## Deleting entire tables
To delete the entire `employeeinfo` table, issue:
```
DROP TABLE IF EXISTS 'employeeinfo';
```
