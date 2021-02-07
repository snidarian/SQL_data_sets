# SQLdatasets
SQL datasets for SQL query practice and safe keeping



### important notes and trade terminology


\
\
\
\

## SQL Quick Reference Sheet


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










