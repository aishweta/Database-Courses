## 1. Introduction
- SQLite is an Open Source software and works on many platforms/OS.
- DB Browser for SQL - sqlitebrowser.org/dl 
- Find the instruction for installation as per your os using above link
- DB browser makes it easy to work with SQLite for learning

What is Database?
- A collection of information, usually involving relationships between the data stored in it.

SQL - Structured Query language (originally called SEQUEL)
Clauses:
SELECT - field
FROM   - table 
WHERE  - condition 
ORDER BY etc
and statements should end with ;


## 2.Ask for Data from a Database
### SELECT - SELECT collects information and return to us.

```
SELECT 'Hello World';
```
or
```
SELECT first_name FROM people;
```
' * ' is used ot represent all the fields in a database. 
```
SELECT * FROM people;
```

### WHERE - add criteria/conditions
```
SELECT * 
FROM people
WHERE state_code = 'CA';
```
### adding more criteria
```
SELECT *
FROM people
WHERE state_code = 'CA' and shirt_or_hat='shirt';
```
### operators
- equals to: = or IS
- no equal to: != or IS NOT or <>

Here we've added 'OR' keywords so basically hat people are also coming
```
SELECT first_name, last_name, state_code, shirt_or_hat
FROM people 
WHERE state_code='CA' OR state_code='NY' AND shirt_or_hat='shirt';
```
If we just want to shirt people we should add paranthesis (similary how we use in algebra)
```
SELECT first_name, last_name, state_code, shirt_or_hat
FROM people 
WHERE (state_code='CA' OR state_code='NY') AND shirt_or_hat='shirt';
```

### LIKE'%..' : % characterrepresents the portion of the string to ignore (this is not case sensetive)
```
SELECT first_name, last_name, state_code, shirt_or_hat
FROM people 
WHERE state_code='CA' OR state_code='CO' OR state_code='CT';
```
we could write alternative query, it tells database to match letter 'C' and whatever comes after 'C' return it
```
SELECT first_name, last_name, state_code, shirt_or_hat
FROM people 
WHERE state_code LIKE 'C%';
```
match in between words 
```
SELECT first_name, last_name, state_code, shirt_or_hat
FROM people 
WHERE first_name LIKE '%ON%';
```
names starting with B and ending with n
```
SELECT first_name, last_name, state_code, shirt_or_hat
FROM people 
WHERE first_name LIKE 'B%N';
```

### LIMIT: limitig the return records (first records)
```
SELECT first_name, last_name, state_code, shirt_or_hat, company
FROM people 
WHERE company LIKE '%LLC'
LIMIT 5;
```

### ORDER BY: sort the result of query
```
SELECT first_name, last_name
FROM people 
ORDER BY first_name;
```
ordering with descending
```
SELECT first_name, last_name
FROM people 
ORDER BY first_name DESC;
```
### LENGTH : it returns the length 
get the length of characters for each name
```
SELECT first_name, LENGTH(first_name)
FROM people;
```
### DISTINCT : get unique values
```
SELECT DISTINCT(first_name)
FROM people;
```
### COUNT 
count people from CA
```
SELECT COUNT(*)
FROM  people
WHERE state_code='CA';
```

## 3. Ask for Data from two or more tables

