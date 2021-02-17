
# SQLdatasets
SQL datasets for SQL query practice and safe keeping




### Refreshers notes and terminology
Note: all language specific keywords are capililized as both a convention of the SQL language and for easy recognition when writing more complicated queries.
Note: keywords and clauses (SELECT, LIMIT, ORDER BY, LIKE etc..) are **order-sensitive**

### Pattern searching wildcards:
- **%** percent sign wildcard matches any string of zero of more characters (like asterisk in std grep search)\
- **_** underscore wildcard matches any single character (like period in std grep search)



## SQL Query-formation Quick Reference

---

#### Clause evaluation-precedence hierarchy:

Precedence | Clause | function
---------- | ------- | -------
1 | FROM | Choose and join tables to get base data
2 | WHERE | Filters base data
3 | GROUP BY | Aggregates base data
4 | HAVING | Filters aggregated data
5 | SELECT | Returns final data
6 | ORDER BY | Sorts the final data
7 | LIMIT | Limits returned data to a row count


---

### SELECT statements


#### Terminology
- The output of a SELECT statement is called results or **a result set** as it’s a set of data that results from a query.
- SELECT * - referred to as a  'select star statement'



#### The following is prudent way structuring a select query
Case insensitive so caps is not necessary but multiple lines helps to avoid errors and provide query clarity through conventional and consistent formatting

```
SELECT 
    lastname, 
    firstname, 
    jobtitle
FROM
    employees;
```

Below is an example for a way to create a special column that is the sum result of the multiplication of two other columns
```
SELECT 
    orderNumber, 
    orderlinenumber, 
    quantityOrdered * priceEach
FROM
    orderdetails
ORDER BY 
   quantityOrdered * priceEach DESC;
```

#### AS keyword

The below query creates synonyms for SELECT arguments using the AS keyword

```
SELECT
    employeenumber AS id,
    lastname AS last,
    firstname AS first
FROM
    employees
ORDER BY
    last ASC,
    first ASC;
```

#### DISTINCT clause in SELECT statements
Distinct clause is used to return non duplicate entries in a column
```
SELECT DISTINCT
lastname
FROM
employees
ORDER BY
lastname ASC;
```
a DISTINCT clause can also be used on multiple columns to aquire row results with novel combinations of requested columns. You can use the DISTINCT clause with more than one column. In this case, MySQL uses the combination of values in these columns to determine the uniqueness of the row in the result set.

For example, to get a unique combination of city and state from the customers table, you use the following query:
```
SELECT
DISTINCT city, state
FROM
customers
ORDER BY
state ASC;
```

Below is the same query but without rows containing NULL in the state field.
```
SELECT
DISTINCT city, state
FROM
customers
WHERE state IS NOT NULL
ORDER BY
state ASC;
```

You can use the DISTINCT clause with an aggregate function e.g., SUM, AVG, and COUNT, to remove duplicate rows before the aggregate functions are applied to the result set.

The below query returns a count of all US states that have at least one customer in them.
```
SELECT
COUNT(DISTINCT state)
FROM
customers
WHERE
country = 'USA';
```
#### DISTINCT with LIMIT clause
Note: It appears limit statements must come at the end of the query (or at least after the order by statement)
The following query selects the first five non-null unique states in the customers table.
```
SELECT DISTINCT
state
FROM
customers
WHERE state IS NOT NULL
LIMIT 5;
```

Using the AVG() function with SELECT to find average of all row items in given column
```
SELECT AVG(msrp) FROM products;
```
The above result of **SELECT AVG(msrp) FROM products** is equal to 100.4387 but instead of writing the number in another query to use this figure we can just write it as a subquery. The below query is returning all msrp values that are above the average msrp calculated on all products
```
SELECT
productname,
msrp
FROM
products
WHERE msrp > (SELECT AVG(msrp) from products)
ORDER BY
msrp ASC;
```

Now just for fun's sake let's calculate and return the average of all items that are greater than the original average msrp of all the items.
```
SELECT
AVG(msrp)
FROM
products
WHERE msrp > (SELECT AVG(msrp) FROM products);
```

Using IN operator with subquery and SUM() function
```
SELECT    
	orderNumber, 
	customerNumber, 
	status, 
	shippedDate
FROM    
	orders
WHERE orderNumber IN
(
	 SELECT 
		 orderNumber
	 FROM 
		 orderDetails
	 GROUP BY 
		 orderNumber
	 HAVING SUM(quantityOrdered * priceEach) > 60000
);
```

SUM() function with subquery:
```
SELECT    
	orderNumber, 
	customerNumber, 
	status, 
	shippedDate
FROM    
	orders
WHERE orderNumber IN
(
	 SELECT 
		 orderNumber
	 FROM 
		 orderDetails
	 GROUP BY 
		 orderNumber
	 HAVING SUM(quantityOrdered * priceEach) > 60000
);
```

the outer query uses the IN operator in the WHERE clause to get data from the orders table:
```
SELECT 
    orderNumber, 
    customerNumber, 
    status, 
    shippedDate
FROM
    orders
WHERE
    orderNumber IN (10165,10287,10310);
```

#### LIMIT clause

The limit clause can accept one or two arguments. The arguments are separated by a comma. The first argument is the offset (the initial valued row to select) and the second is the number of rows or "limit" of rows.

If you don't specify the row offset (first arg) then by default it is set to 0 (the first row)
```
LIMIT 0 , row_count;
```
It is always a good practice to use the ORDER BY clause and LIMIT clause together for one reason: Unless you order the rows that are returned, if you use a LIMIT clause with N rows being returned you have no idea which N rows are going to be returned unless they are ordered with ORDER BY
```
SELECT
employeenumber, lastname
FROM
employees
ORDER BY
employeenumber
LIMIT 0, 10
```

The following query returns the customers who possess the highest credit limit. By using ORDER BY to DESC (order by descending order) the rows based on the creditlimit column you are specifically asking SELECT and LIMIT for the rows with the highcredit.
```
SELECT 
    customerNumber, 
    customerName, 
    creditLimit
FROM
    customers
ORDER BY creditLimit DESC
LIMIT 5;
```
The same can be done to find the customers with the lowest credit:
```
SELECT
customername,
creditlimit
FROM
customers
ORDER BY
creditlimit ASC
LIMIT 0, 8;
```
##### COUNT() function:
The below query finds out how many rows are in the customers table
```
SELECT COUNT(*) FROM customers;
```
returns:
```
+----------+
| COUNT(*) |
+----------+
|      122 |
+----------+
1 row in set (0.00 sec)
```

##### Using LIMIT for pagination

This query uses the LIMIT clause to get rows of page 1 which contains the first 10 customers sorted by the customer name:
```
SELECT 
    customerNumber, 
    customerName
FROM
    customers
ORDER BY customerName    
LIMIT 10;
```

This query uses the LIMIT clause to get the rows of the second page that include row 11 – 20:
```
SELECT 
    customerNumber, 
    customerName
FROM
    customers
ORDER BY customerName    
LIMIT 10, 10;
```

##### using LIMIT to get the Nth highest or lowest value

The highest of lowest value (using ORDER BY to sort the rows of course) will be easiest to access as the first value when ordering by ASC or DESC. For instance the customer with the highest credit limit can be accessed thusly using the ordering scheme and the offset value of the LIMIT clause:

```
SELECT
customername
creditlimit
FROM
customers
ORDER BY
creditlimit DESC
LIMIT 0,1;
```
To get the customer with the lowest credit you would just reverse the ordering scheme:
```
SELECT
customername,
creditlimit
FROM
customers
ORDER BY
creditlimit ASC
LIMIT 0, 1;
```
In this same way you could easily find the customer with the second highest credit:
```
SELECT
customername,
creditlimit
FROM
customers
ORDER BY
creditlimit DESC
LIMIT 1,1;
```

---

### WHERE keyword

WHERE clause in the SELECT statement filters rows from the result set. Besides the SELECT statement, you can use the WHERE clause in the UPDATE or DELETE statement to specify which rows to update or delete. In the SELECT statement, the WHERE clause is evaluated after the FROM clause and before the SELECT clause.

```
SELECT 
    lastname AS last, 
    firstname, 
    jobtitle
FROM
    employees
WHERE
    jobtitle = 'Sales Rep'
ORDER BY
    last ASC;
    
```

#### AND, OR, NOT - Logical Operators
WHERE statements can employ the logical operators: 
- AND
- OR
- NOT

The below query finds rows where the amount is more than $3000 and customernumber is between 400 and 500
```
SELECT
customernumber
amount
FROM
payments
WHERE
amount > 3000 AND customernumber > 400 AND customernumber < 500;
```
Another example of using AND logical operator. This time to find customers from CA, USA.
```
SELECT
country,
state,
customername
WHERE
country = 'USA' AND state = 'CA'
ORDER BY
state ASC;
```
OR
```
SELECT
country,
state,
customername
FROM
customers
WHERE
country = 'uk' or state = 'ca'
ORDER BY
country ASC;
```
NOT

```
SELECT
country, state, customername
FROM
customers
WHERE
country = 'usa' AND NOT state = 'ca'
ORDER BY
state ASC;
```

Note: in the below query that since WHERE is evaluated before before SELECT using code as a synonym defined in the SELECT statement will return an error as the SELECT statement has not yet been evaluated by the time the WHERE statement is evaluated and so the synonym doesn't yet exist.
```
SELECT
    firstname,
    employeenumber AS id,
    officecode AS code
FROM
    employees
WHERE
    jobtitle = 'sales rep' AND officecode = 1 
ORDER BY 
    id ASC;
```

#### BETWEEN operator - The BEWTEEN operator is a logical operator

BETWEEN operator can be used with WHERE and logical operators AND
BETWEEN returns True if value is in a range of numbers.

```
SELECT
firstname,
lastname,
officecode
FROM
employees
WHERE
officecode BETWEEN 1 AND 3
ORDER BY
officecode;
```

Below example uses multiple logical operators in WHERE statement
```
SELECT
productname,
msrp
FROM
products
WHERE
msrp BETWEEN 30 AND 50 OR msrp BETWEEN 70 AND 80
ORDER BY
msrp ASC;
```
Another BETWEEN example use

```
SELECT 
ordernumber,
productcode, 
quantityordered 
FROM orderdetails 
WHERE 
quantityordered BETWEEN 20 AND 28 
ORDER BY 
quantityordered DESC;
```
##### BETWEEN example - Returning rows bases on value in a range
```
SELECT
customernumber,
amount
FROM
payments
WHERE amount BETWEEN 12000 AND 45000
ORDER BY
amount;
```
##### NOT BETWEEN example - Returning rows bases on value outside of a range
```
SELECT
customernumber,
amount
FROM
payments
WHERE
amount NOT BETWEEN 12000 AND 50000
ORDER BY
amount DESC;
```
Using BETWEEN with Dates -When you use the BETWEEN operator with date values, to get the best result, you should use the type cast to explicitly convert the type of column or expression to the DATE type.
The following example returns the orders which have the required dates between 01/01/2003 to 01/31/2003:
```
SELECT 
   orderNumber,
   requiredDate,
   status
FROM 
   orders
WHERE 
   requireddate BETWEEN 
     CAST('2003-01-01' AS DATE) AND 
     CAST('2003-01-31' AS DATE);
```
Because the data type of the required date column is DATE so we used the CAST operator to convert the literal strings '2003-01-01' and '2003-12-31' to the DATE values.

---

#### LIKE operator in WHERE statements:\

##### Two wildcard characters for constructing something analogous to regex in MySQL
- **%** - equivalent to asterisk in unix terms - matches any string of zero of more chars.
- **_** - equivalent to period in unix terms - matches any single character.


The below search pattern could also be written as '%s_n'\
Note that if you took away the 'son' in '%son' and only included percent sign it would return every lastname in table.
```
SELECT
firstname,
lastname
FROM
employees
WHERE lastname LIKE '%son'
ORDER BY
firstname;
```

Since Ford we're looking for wherever the word 'Ford' appears we surround it with wildcard characters. '%ford%' searches for wherever the word appears in productname
```
SELECT 
productname, 
msrp 
FROM 
products 
WHERE 
productname LIKE '%ford%' 
ORDER BY msrp ASC;
```

Using underscore wildcard to find name that begins with 'T' and ends with 'm'
```
SELECT 
    employeeNumber, 
    lastName, 
    firstName
FROM
    employees
WHERE
    firstname LIKE 'T_m';
```

Using NOT LIKE to return all results except what the regex matches.
```
SELECT 
    employeeNumber, 
    lastName, 
    firstName
FROM
    employees
WHERE
    lastName NOT LIKE 'B%';
```

#### LIKE Operator with ESCAPE clause

using the ESCAPE keyword you can specify a character to use as an escape character.
If you don't specify an escape character with ESCAPE '[CHAR]' then the default escape character is the backslash char.

Example:
```
SELECT 
    productCode, 
    productName
FROM
    products
WHERE
    productCode LIKE '%\_20%';
```

I could make that exact same query again and this time define '$' as the escape character:
```
SELECT 
    productCode, 
    productName
FROM
    products
WHERE
    productCode LIKE '%$_20%' ESCAPE '$';
```

---


#### IN operator in WHERE statements:\

IN returns true if value matches any value in a list
```
SELECT 
officecode, 
phone,
addressline1 
FROM 
offices 
WHERE 
officecode IN (1, 2, 3) 
ORDER BY 
officecode ASC;
```
Another example of IN operator
```
SELECT 
    officeCode, 
    city, 
    phone, 
    country
FROM
    offices
WHERE
    country IN ('USA' , 'France');
```
the NOT operator can be used with the IN operator as seen below

```
SELECT
officecode,
city,
phone,
country
FROM
offices
WHERE
country NOT IN ('usa' , 'france');
```
#### IN operator with Subqueries
```
SELECT    
	orderNumber, 
	customerNumber, 
	status, 
	shippedDate
FROM    
	orders
WHERE orderNumber IN
(
	 SELECT 
		 orderNumber
	 FROM 
		 orderDetails
	 GROUP BY 
		 orderNumber
	 HAVING SUM(quantityOrdered * priceEach) > 60000
);
```

#### IS NULL operator in WHERE statement
```
SELECT
firstname,
lastname,
reportsto
FROM
employees
WHERE reportsto IS NULL;
```

Compatible operators for use in WHERE statements

| Operator | Description |
| ---------|-------------|
| = | Equal to. You can use it with almost any data types. |
| <> or != | Not equal to |
| < | Less than. You typically use it with numeric and date/time data types. |
| > | Greater than. |
| <= | Less than or equal to |
| >= | Greater than or equal to |




---
### ORDER BY - keyword

The below query returns a result set that orders col1 in ascending order and col2 in descending order
Example:
```
SELECT 
col1, col2
FROM
example_table
ORDER BY
col1 asc,
col2 desc;

```
Note: What it's specifically doing is tell the result set to be sorted in terms of ascending order in col1 first and then descending order of col2 afterwards.
In ORDERY BY, the order of which columns are ordered first matters. Note also that the ORDER BY clause is always evaluated after the FROM and SELECT clause.

Example from sample data set 'classicmodels' customer table:
```
SELECT
contactlastname, 
contactfirstname 
FROM 
customers 
ORDER BY 
contactlastname asc, 
contactfirstname desc;

```





