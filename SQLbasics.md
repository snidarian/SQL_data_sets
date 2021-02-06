### SQL is a language used to interact with 'relational database management systems'

A database is any collection of related information (a handwritten shopping list is a crude version of a shopping list)

The database management system (dbms) is not the database; It is the software that manages the database.

-------------------------------------------------------

> -- Write comments in SQL by using two dashes '--'
> /* Write block comments in SQL by using the asterisk slash syntax used in C and Javascript */

## A Database Management System:
- makes it easy to manage large amounts of data
- Handles security
- Backups the data
- facilitates importing and exporting data
- Concurrency
- interacts with software applications (programming languages)

----------------------------------------------------------------



## C.R.U.D.
- Create
- READ (or retrieve)
- Update
- Delete

> CRUD represents the four main operations that are performed on a database.


---------------------------------------------------

## TWO MAIN TYPES OF DATABASES
1. Relational Databases (SQL)
2. Non-Relational Databases (noSQL/not just SQL)


### RELATIONAL DATABASES
- By far the most popular
- Oranizes data into one of more tables
- A relational database is kind of like an excel spreadsheet
    - Each table has columns and rows
    - a unique key identifies each row


### NON-RELATIONAL DATABASES
- Any type of database that is not a relational database
- Organizes data in anything but a traditional table format
    - Key-value stores
    - Documents that store data like JSON, XML, CSV
    - Graphs
    -Flexible tables 

-------------------------------------


Structured Query Language - a DBMS uses SQL, a standardized language for interacting with relational database management systems
- Standardized language for interacting with RDBMS
- Used to perform CRUD operations as well as other administrative tasks (user management, security, backup, etc)
- Used to define tables and structures
- SQL code used on one RDBMS is not always portable to another without modification


NOT ALL SQL LANGUAGES ARE CREATED EQUAL OR IMPLEMENTED THE SAME (but they are all VERY similar.) Be careful in assuming portability in one SQL language to the other

--------------------------------------

QUERIES - A query is a request for information from the database

Think of a query as a google search (and indeed there are a lot of similarities)
- both are used in order to obtain information
- both require a specific protocol of syntax (in the case of a google search words should be spelled correctly and proper terminology used)
- both are used to communicate with a database that is holding the information



--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
CORE CONCEPTS of relational databases


Whenever we create a table in a relation database we always want to have special column called a "**Primary Key** (column)". The primary key serves as a key indentifier for that specific row


> YOU ALWAYS  WANT TO DEFINE A PRIMARY KEY (A primary key can be any data type BUT it has to uniquely identify it's row with respect to all the other rows)

### Key types

1. Surogate key - a primary key that has no mapping to the real world (meaning that is more or less an arbitrary number that has no significance outside our database)

2. Natural key - A primary key that DOES have a mapping to the real world (meaning it has some significance outside our database ie: a social security number)

3. Foreign key - a attribute that stores the primary key in a row of another database.

4. Composite key - a primary key that requires two attributes (meaning that it is made up of two columns) 
   - A composite key is needed when one primary key alone cannot be used by itself to uniquely identify each row.

Sometimes a primary key is define as two foreign keys (meaning the two primary keys of two other tables)


> Here's an example (written in markdown) of the concept of a primary key

| Employee ID (Primary Key) | Name | salary | Job description |
| --- | --- | --- | --- |
| 101 | John | 100k | Corporate exec |
| 234 | Christina | 55k | Secretary |
| 340 | Calvin | 76K | Store manager | 


In the above example the primary key is the employee ID that identifies the employeed uniquely from all the others.
Employee IDs are an example of a 'surogate key' meaning that it has no critical real world significance like a Social security number would for instance. 

-----------------------------------------------------------------------------------------------

BASICS OF SQL - (SQL is not a programming language in the traditional sense but i can be used to provide instructions to a RDBMS)

Not all RDBMNS follow the SQL standard to a 'T' - So the concepts are all the same but the implementations mary vary between RDBMS

SQL is a hybrid language - It's four languages in one
- Data query language
    - used to query db for information
    - getting information that's already stored there
- Data definition language
    - used for defining database schemas
- Data control language
    - meaning that it's used to control access the information (permissions, users)
- Data manipulation language
    - Used for inserting, updating, deleting data from the database
