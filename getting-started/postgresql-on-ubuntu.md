## Overview##
This guide covers how to install PostgreSQL (PgSQL) server on your new Ubuntu and introduces basic PgSQL commands. We recommend you complete the following steps as a limited sudo user. For more information about setting up limited sudo users, see this article.
## Installing PgSQL##
Using the package manager, perform a system update to ensure you are running the most current version of Ubuntu:
```
sudo apt-get update
sudo apt-get install postgresql postgresql-contrib
```
## Getting started with PgSQL
After installation, you must connect to the Postgres server. PgSQL creates a new user on installation called postgres, which you will use to connect. 

To connect:
1. Switch to the Postgres user before using the *`psql`* command.
```
sudo -i -u postgres
psql
```
You should see the following prompt:
```
psql (9.6.2)
Type "help" for help.

postgres=#
```
2. To quit, input:
```
/q
```
Or, you can execute the *`psql`* command with sudo and without changing users with:
```
sudo -u postgres psql
```
##Databases and roles
PgSQL does not use user and group ownership like a traditional Unix-style account. Rather, it uses the term roles as an all-encompassing term. A role can be either a specific user or a larger group, depending on how it is set up. On our new installation of PgSQL, there is only one role, `postgres`. You can see this from the psql prompt with the *`\du`* command in the below example:
```
psql (9.6.2)
Type "help" for help.

postgres=# \du
                                   List of roles
 Role name |                         Attributes                         | Member of 
-----------+------------------------------------------------------------+-----------
 postgres  | Superuser, Create role, Create DB, Replication, Bypass RLS | {}
```
We can do a similar check for our existing databases with the *`\l`* or *`\list`* command:
```
postgres=# \l
                                  List of databases
   Name    |  Owner   | Encoding |   Collate   |    Ctype    |   Access privileges   
-----------+----------+----------+-------------+-------------+-----------------------
 postgres  | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | 
 template0 | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =c/postgres          +
           |          |          |             |             | postgres=CTc/postgres
 template1 | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =c/postgres          +
           |          |          |             |             | postgres=CTc/postgres
(3 rows)
```
## Creating new databases and roles
Next, this guide demonstrates how to create a new database, a new role, and assign the new role to that database. 
**Attention:**These commands need to be ran as the `postgres` user on the linux system, not from within psql.
1. Create a new role to later be added to the new database. The `--pwprompt` option will ask you to set the user’s password.
```
createuser testrole --pwprompt
```
2. Create a new database. Again, this should be ran from the main Linux prompt, not from within psql:
```
createdb test db
```
3. Populate the database with a simple table and grant the new role permissions on that table:
   a. Connect to the new database:
   ```
   psql testdb
   ```
   b. Once on the new prompt for psql, create a new table with some basic parameters and insert some data:
   ```
   CREATE TABLE customers (customer_id int, first_name varchar, last_name varchar);
   INSERT INTO customers VALUES (1, ‘Jane’, ‘Schmuckatelli’);
   ```
   c. Grant the new role all privileges to the new table:
   ```
   GRANT ALL ON customers TO testrole;
   ```
## Asssigning roles to groups
A role can alternatively be used as a group. You can grant privileges to the group for ease of management and then assign roles to this group.

To start, from the Linux command prompt, create a new user. The `--no-login` flag is specified here, as groups do not need to log in to the pgsql server. 
```
createuser testgroup --no-login
```
Next, login to the postgres server and add our previous role to the new group role:
```
psql postgres
GRANT testgroup TO testrole;
```
## Cleaning up
It’s worthwhile to practice “good housekeeping” by removing all of your recent changes. 
1. To start, remove the testrole from the testgroup:
```
REVOKE testgroup FROM testuser;
```
2. Connect to the testdb and revoke privileges from the test role:
```
\c testdb
REVOKE ALL ON customers FROM testrole;
```
3. Delete our data and then remove the table:
```
DELETE FROM customers WHERE customer_id = 1;
DROP TABLE customers;
```
4. Remove the user and database. These last commands will be executed from the Linux prompt:
```
dropuser testrole
dropdb testdb
```
