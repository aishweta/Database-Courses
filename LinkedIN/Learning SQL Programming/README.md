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

### JOIN
we are getting 5000 records 
```
SELECT first_name, state_code
FROM people
JOIN states;
```
get state_code associated information from both the tables (below query returns 1000 records) 
```
SELECT first_name, state_code
FROM people
JOIN states on people.state_code=states.state_abbrev;
```
get all the fields from both the tables associated with state_code and state_abbrev
```
SELECT *
FROM people
JOIN states on people.state_code=states.state_abbrev;
```
conditions on both the tables - get the table where people names startes with J and region ins South
```
SELECT *
FROM people
JOIN states on people.state_code=states.state_abbrev
WHERE people.first_name LIKE 'J%' AND states.region='South';
```
##### Asign abbreviations to the table names
tables people as 'p' and table states as 's'
```
SELECT pp.first_name , s.state_abbrev
FROM people pp , states s
JOIN states on pp.state_code=s.state_abbrev;
```

### Cros join
get every result of right and left table
```
SELECT *
FROM people JOIN states;
```

### Inner join
overlaping results, it just returns matched results and excludes other values
```
SELECT * FROM people JOIN states
ON people.state_code=states.state_abbrev;
```
### (LEFT) OUTER join
overlaping results, it just returns every results of left table (matched results for all the values in left table and where we don't have matched values associated with left table it adds null values)
```
SELECT * FROM people LEFT JOIN states
ON people.state_code=states.state_abbrev;
```
### GROUP BY
group by with count on first_name
```
SELECT first_name, COUNT(st_name)
FROM people
GROUP BY first_name;
```
another example
```
SELECT state_code, quiz_points, COUNT(quiz_points)
FROM people
GROUP BY state_code, quiz_points;
```
### Complicated query (Excercise)
```
SELECT states.state_name, COUNT(people.shirt_or_hat)
FROM states
JOIN people ON states.state_abbrev=people.state_code
WHERE people.shirt_or_hat = 'hat'
GROUP BY people.shirt_or_hat, states.state_name;
```

## 4. Data types, Math and helpful feature
- The VARCHAR type is used for storing a variable number of characters.
### math
- standard operators: +, - , *, / and %
- comparision operators: <,>,<>,!=,=,>=,<=
- calculation functions: SUM(), COUNT(), AVG(), MAX() and MIN()
```
SELECT 1/3.0
```
on database
```
SELECT first_name, quiz_points
FROM people
WHERE quiz_points>80;
```
MAX and Min
```
SELECT MAX(quiz_points) , MIN(quiz_points)
FROM people;
```
more complicated
```
SELECT team, COUNT(*) , SUM(quiz_points), SUM(quiz_points)/COUNT(*)
FROM people
GROUP BY team
```

### Compound select
```
SELECT first_name, last_name , quiz_points
FROM people
WHERE quiz_points=(SELECT MAX(quiz_points) FROM people);
```

### Transforming data
```
SELECT LOWER(first_name), upper(last_name)
FROM people;
```

### substring - ( string , character_start_no , character_after_start_no)
```
SELECT LOWER(first_name), substr(last_name, 2, 4)
FROM people;
```
last 2 letters of the string
```
SELECT LOWER(first_name), substr(last_name, -2)
FROM people;
```

### replace string
```
SELECT LOWER(first_name), replace(last_name, "a", "-")
FROM people;
```

### change data type
```
SELECT first_name, quiz_points
FROM people 
ORDER BY CAST(quiz_points AS CHAR);
```
test using another example (in output max value in '99'
```
SELECT MAX(CAST(quiz_points AS CHAR))
FROM people 
```

### AS - create aliases
```
SELECT first_name AS name
FROM people
```

## 5. Add or Modify data
### INSERT INTO
below statement will add one value in last row of the table
```
INSERT INTO people (first_name) VALUES ('Bob')
```
insert multiple values
```
INSERT INTO people 
(first_name, last_name, state_code, city, shirt_or_hat) 
VALUES
('Bob', 'Hamilton', 'NY','NEW YORK CITY', 'hat')
```

### UPDATE
update specific value in db 
```
UPDATE people
SET last_name='Morrison' WHERE first_name='Carlos'
```

### REMOVE data
```
DELETE FROM people
WHERE id_number=1001;
```

### check NULL
```
SELECT * FROM people
WHERE quiz_points IS NULL
```

### common mistakes
- typo and syntax error
- text value should be in single quote

