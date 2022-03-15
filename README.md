# test_db
## Notes
Since you have both a root user and your computer's password protected user, you might have to include -p when you are trying to import the sample database. Examples:

    mysql -p < employees.sql 
    Or
    mysql -u root -p < employees.sql
    For testing the tables
    mysql -p -t < test_employees_md5.sql

### WHERE, LIKE, DISTINCT, BETWEEN, IN
The WHERE clause, if given, indicates the condition or conditions that rows must satisfy to be selected. The WHERE condition is an expression that evaluates to true for each row to be selected.

We can use WHERE with the LIKE option to find similarities. The % are wildcards.
This query will select all first names with the letter combination 'sus':

    SELECT DISTINCT first_name
    FROM employees
    WHERE first_name LIKE '%sus%';
* Can add the DISTINCT keyword to our SELECT statement to avoid getting duplicate values.

BETWEEN to find specific ranges of values:
    
    WHERE emp_no BETWEEN 10026 AND 10082;
We can use WHERE with IN to query only very specific sets of values. The () are required when you use IN.

    WHERE last_name IN ('Herber', 'Dredge', 'Lipner', 'Baek');

We can also use comparison operators, IS NULL, IS NOT NULL.

We can chain using AND or OR:

    WHERE emp_no < 20000
        AND last_name IN ('Herber','Baek')
        OR first_name = 'Shridhar';


<hr>

### Setup Schema/DB

1. Open the database tool window
2. View -> Tool Windows -> Database
3. Add a new connection
4. Click on the '+' and choose Data Source -> MySQL
5. Some versions of IntelliJ might require to specify the server version while you choose a driver, in your case could be something like: MySQL 8.x.x
6. Fill in the following:


    Host: localhost
    Port: 3306
    Database:
    User: root
    Password: codeup (or your root password)

Note that you should leave the "Database" field blank so you can see them all.
7. Under the Advanced tab, find the serverTimezone property and set it to UTC
8. Click Test Connection and save if successful

<hr>

## Exercises

### Exercise 1
Select all from the employees tables. You should see 300,024 results returned.
    
    SHOW DATABASES;
    USE employees;
    SHOW TABLES;
    pager less -~SFX
    SELECT * FROM employees;
    npager;

### Exercise 2
Expand the @localhost then expand employees_db

You may need to click "Schemas..." to tell IntelliJ which databases you wish to work with on this project.

Run describe departments; from the query window. Compare this to the data you see when you expand the departments table in the Database Tool Window.

In the query window, craft a query that will show all the records in the departments table. You should see 9 rows returned in the view below.

Explore the structure of each table.

### Exercise 3


<hr>

# Description
A sample database with an integrated test suite, used to test your applications and database servers

This repository was migrated from [Launchpad](https://launchpad.net/test-db).

See usage in the [MySQL docs](https://dev.mysql.com/doc/employee/en/index.html)


## Where it comes from

The original data was created by Fusheng Wang and Carlo Zaniolo at 
Siemens Corporate Research. The data is in XML format.
http://timecenter.cs.aau.dk/software.htm

Giuseppe Maxia made the relational schema and Patrick Crews exported
the data in relational format.

The database contains about 300,000 employee records with 2.8 million 
salary entries. The export data is 167 MB, which is not huge, but
heavy enough to be non-trivial for testing.

The data was generated, and as such there are inconsistencies and subtle
problems. Rather than removing them, we decided to leave the contents
untouched, and use these issues as data cleaning exercises.

## Prerequisites

You need a MySQL database server (5.0+) and run the commands below through a 
user that has the following privileges:

    SELECT, INSERT, UPDATE, DELETE, 
    CREATE, DROP, RELOAD, REFERENCES, 
    INDEX, ALTER, SHOW DATABASES, 
    CREATE TEMPORARY TABLES, 
    LOCK TABLES, EXECUTE, CREATE VIEW

## Installation:

1. Download the repository
2. Change directory to the repository

Then run

    mysql < employees.sql


If you want to install with two large partitioned tables, run

    mysql < employees_partitioned.sql


## Testing the installation

After installing, you can run one of the following

    mysql -t < test_employees_md5.sql
    # OR
    mysql -t < test_employees_sha.sql

For example:

    mysql  -t < test_employees_md5.sql
    +----------------------+
    | INFO                 |
    +----------------------+
    | TESTING INSTALLATION |
    +----------------------+
    +--------------+------------------+----------------------------------+
    | table_name   | expected_records | expected_crc                     |
    +--------------+------------------+----------------------------------+
    | employees    |           300024 | 4ec56ab5ba37218d187cf6ab09ce1aa1 |
    | departments  |                9 | d1af5e170d2d1591d776d5638d71fc5f |
    | dept_manager |               24 | 8720e2f0853ac9096b689c14664f847e |
    | dept_emp     |           331603 | ccf6fe516f990bdaa49713fc478701b7 |
    | titles       |           443308 | bfa016c472df68e70a03facafa1bc0a8 |
    | salaries     |          2844047 | fd220654e95aea1b169624ffe3fca934 |
    +--------------+------------------+----------------------------------+
    +--------------+------------------+----------------------------------+
    | table_name   | found_records    | found_crc                        |
    +--------------+------------------+----------------------------------+
    | employees    |           300024 | 4ec56ab5ba37218d187cf6ab09ce1aa1 |
    | departments  |                9 | d1af5e170d2d1591d776d5638d71fc5f |
    | dept_manager |               24 | 8720e2f0853ac9096b689c14664f847e |
    | dept_emp     |           331603 | ccf6fe516f990bdaa49713fc478701b7 |
    | titles       |           443308 | bfa016c472df68e70a03facafa1bc0a8 |
    | salaries     |          2844047 | fd220654e95aea1b169624ffe3fca934 |
    +--------------+------------------+----------------------------------+
    +--------------+---------------+-----------+
    | table_name   | records_match | crc_match |
    +--------------+---------------+-----------+
    | employees    | OK            | ok        |
    | departments  | OK            | ok        |
    | dept_manager | OK            | ok        |
    | dept_emp     | OK            | ok        |
    | titles       | OK            | ok        |
    | salaries     | OK            | ok        |
    +--------------+---------------+-----------+


## DISCLAIMER

To the best of my knowledge, this data is fabricated and
it does not correspond to real people. 
Any similarity to existing people is purely coincidental.


## LICENSE
This work is licensed under the 
Creative Commons Attribution-Share Alike 3.0 Unported License. 
To view a copy of this license, visit 
http://creativecommons.org/licenses/by-sa/3.0/ or send a letter to 
Creative Commons, 171 Second Street, Suite 300, San Francisco, 
California, 94105, USA.


