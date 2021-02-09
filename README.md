# SQLdatasets
SQL datasets for SQL query practice and safe keeping



### Refreshers notes and terminology

### Pattern searching wildcards:
- **%** percent sign wildcard matches any string of zero of more characters (like asterisk in std grep search)\
- **_** underscore wildcard matches any single character (like period in std grep search)

\
\
\

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
WHERE statements can employ the logical operators: 
- AND
- OR
- NOT

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

#### BETWEEN operator

BETWEEN operator can be used with WHERE and logical operator AND
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

#### LIKE operator in WHERE statements:\

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

Operator | Description
---------|-------------
= | Equal to. You can use it with almost any data types.
<> or != | Not equal to
< | Less than. You typically use it with numeric and date/time data types.
> | Greater than.
<= | Less than or equal to
>= | Greater than or equal to




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










