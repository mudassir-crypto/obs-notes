
>A db is a collection of data, a method for accessing and manipulating the data

>Database is a software or hardware that allows user to store, organise and use data

>The software that is utilised to manage databases - DBMS

>DBMS not only does the crud operation but also maintains security and make sure the interactive user does the right things

#### Types of Database: 
  * Relational - postgres, microsoft sql server, oracle
  * Document Model - Mongodb, firebase
  * Key Value - Redis
  * Graph - For data that should be connected in different ways
  * Wide Columnar

>SQL - A language to interact with a db
>Query - Instructions

#### SQL is a declarative language:

  Declarative                                              Imperative (c++, etc)

  What will happen                                    How it will happen

  We have no idea how it will happen

DB Types - Hierarchical(like html tags) & Networking, Relational 
Table is a collection of rows(tuples) and columns(attributes)

  
>OLTP vs OLAP

  Online Transaction Processing   vs    Online Analytical Processing
         Day to day Transaction                    Supports analysis
         

Creating and inserting values into the table:

 ```sql
 CREATE TABLE User (
    id varchar(255) NOT NULL,
    name varchar(255) NOT NULL,
    dob varchar(255) NOT NULL,
    sex varchar(1) NOT NULL,
    role varchar(255) NOT NULL,
    PRIMARY KEY (id)
  );

 INSERT INTO User
 VALUES ('u1', 'Tate', '1992-01-19', 'm', 'employee');

 INSERT INTO User
 VALUES ('u2', 'Tina', '1992-02-19', 'f', 'manager');

 INSERT INTO User
 VALUES ('u3', 'John', '1992-01-19', 'm', 'manager');
 
 INSERT INTO User
 VALUES ('u4', 'Jana', '1992-01-19', 'f', 'employee');
 ```

renaming columns:
 ```sql
 SELECT column as '<new_name>'
 ```

  
concatenate two columns:
 ```sql
 SELECT CONCAT(emp_no, ' ', title) as emp from titles
 SELECT CONCAT(emp_no, ' is a ', title) as emp from titles
 ```
  

Scalar functions:
  Takes input of a single row

Aggregate function:
  Aggregate functions compute a single result from a set of input values (Takes input of all the rows)

 >min, max, avg, sum, count
  
  Aggregate functions are not allowed in where clause


SELECT:
 ```sql
 SELECT * from salaries WHERE salary=(SELECT max(salary) from salaries);

 SELECT * from employees WHERE birth_date=(SELECT min(birth_date) from employees);

 SELECT * from employees WHERE first_name='Mayumi' And last_name='Schueller';

 -- SELECT * from employees WHERE (state='OR' OR state='NY') AND gender='F'

 SELECT * from employees WHERE NOT age=55;

 SELECT e.emp_no, s.salary from employees as e 
 inner join salaries as s using(emp_no)
 order by s.salary desc
 limit 1 offset 3  --limit(how many rows) offset (from which row to start from)
 ``` 

Logical Operator:
  `AND, OR, NOT`

Comparison Operator:
 ` =, >, <, >=, <=, <> or !=`

Operator Precedence:
  A statement having multiple operator is evaluated based on the priority of operators
  `Parenthesis -> */% -> +- -> Not And Or`

  If the operator has equal precedence, then the operators evaluated from left to right or right to left

 ```sql
 SELECT firstname, age, income, country from customers
 WHERE income>50000 AND (age<30 OR age>50) AND (country='Japan' OR country='Australia')
  
 SELECT * from orders
 WHERE (orderdate>='2004-04-01' AND orderdate<='2004-04-30') And totalamount>100
 ```

NULL:
  Any operation on null value gives null
  Always check for null when necessary
  Comparison operator cannot be used to find null values, Instead we use IS operator
```sql
SELECT * from User WHERE name IS NULL;
SELECT * from User WHERE name IS NOT NULL;
```


replace null values of column with a given value:
 ```sql
 SELECT coalesce(name, 'no name available') from User

 SELECT id, coalesce(name, 'no name available') as name, age from User;

 SELECT id, coalesce(age, 52) as age, name from User;

 SELECT id, coalesce(name, email, 'no name or email available') as name, age from User;
 
 SELECT avg(coalesce(age, 15)) from "Student";

 SELECT id, coalesce(name, 'default'), coalesce(lastname, 'lastname'),age from "Student";
 ```

BETWEEN AND:  shorthand to match against the range of values
 ```sql
 SELECT * from <table> WHERE <column> BETWEEN x AND y
 ```

IN: It is like an OR statement, check if a field matches any value
 ```sql
 SELECT * from <table> WHERE <column> IN (val1, val2, ....)
 ```

Casting:
 ```sql
 CAST(salary AS text)
 salary::text
 ```

LIKE:   this operator only works on text so cast it if it is of another type
ILIKE: for case insensitive match

 ```sql
 SELECT * from customers WHERE zip::text LIKE '%2%'

 SELECT * from customers WHERE name LIKE 'Gr%'

 SELECT emp_no, first_name, EXTRACT (YEAR FROM AGE(birth_date)) as "age" FROM employees
 WHERE first_name ILIKE 'M%';
 ```
  
Timezones & date:
 ```sql
 SHOW TIMEZONE;

 SET TIME ZONE 'UTC'; (it sets the timezone for only tht session)

 ALTER USER postgres SET timezone='UTC';
 ```
 

 Always use UTC:
  postgres uses ISO-8601 standard for the formats of date and time
  YYYY-MM-DDTHH:MM:SS

Current date:
 ```sql
 SELECT now()::date;

 SELECT CURRENT_DATE;

 SELECT TO_CHAR(CURRENT_DATE, 'dd/mm/yyyy') --defining a format for the current date

 SELECT TO_CHAR(CURRENT_DATE, 'dd') 

 Difference: Subtracting dates returns the difference in days:
 SELECT now() - '1800-09-23'

 SELECT date '1800/01/01';  => 1800-01-01
 ```
 
Age calculation:
 ```sql
 SELECT AGE('1800-01-01') // wrong, cast it into date

 SELECT AGE(date '1800-01-01')

 SELECT AGE(date '2001-04-14', '1900-01-01') // difference between them

 SELECT emp_no, first_name, EXTRACT (YEAR FROM AGE(birth_date)) as "age" FROM employees
 WHERE first_name ILIKE 'M%'; // in place of YEAR, we can use DAY, MONTH

 SELECT EXTRACT (MONTH from date '1900-01-01') AS month
 ```
  
Rounds a date:
```sql
 SELECT DATE_TRUNC('year', date '1900-04-26') AS year => 1900-01-01

 SELECT DATE_TRUNC('month', date '1900-04-26') AS year => 1900-04-01
 ```

Interval: Stores and manipulate a period of time in years, months, days, hours, minutes, second
 ```sql
 INTERVAL '1 year 2 months 3 days'

 INTERVAL '2 weeks ago'

 INTERVAL '1 year 3 hours 20 minutes'

 SELECT EXTRACT(YEAR from INTERVAL '24 months');

 select * from orders purchaseDate <= now() - interval '30 days'
```


Get me all the employees above 60, use the appropriate date functions
 ```sql
  SELECT *, age(birth_date) from employees
  WHERE (EXTRACT (year from age(birth_date))) > 60;
 ```

How many employees were hired in February:
 ```sql
 SELECT * from employees
 WHERE EXTRACT (MONTH from hire_date) = 2;
 ```

How many employees were born in november: 
 ```sql
 SELECT * from employees
 WHERE EXTRACT (MONTH from birth_date) = 11;
 ```

Who is the oldest employee? (Use the analytical function MAX)
 ```sql
 SELECT MAX(AGE(birth_date)) FROM employees
 ```

How many orders were made in January 2004?
 ```sql
 SELECT * from orders
 WHERE date_trunc('month', orderdate) = date '2004-01-01'
 ```


DISTINCT: eliminates duplicate data
 ```sql
 SELECT DISTINCT birth_date from employees;
 ```

Order By: sorting the data in ascending or descending order
 ```sql
 SELECT emp_no, first_name, last_name from employees
 ORDER BY first_name, last_name DESC
 
 SELECT emp_no, first_name, last_name from employees
 ORDER BY length(first_name) DESC
```

Multiple SELECT statement:
 ```sql
  SELECT e.first_name, e.last_name, s.salary from employees as e, salaries as s
  WHERE e.emp_no = s.emp_no;
```
  
Inner Join: Retrieves the matching rows
 ```sql
 SELECT e.emp_no, CONCAT(e.first_name, ' ' ,e.last_name) as name, s.salary from employees as e
  INNER JOIN salaries as s
  ON e.emp_no = s.emp_no ORDER BY e.emp_no;
```
 
3 tables chaining:
 Give salary when there title changed
 ```sql
 SELECT e.emp_no, s.salary, s.from_date, t.title from employees as e
 INNER join salaries as s on e.emp_no = s.emp_no
 INNER join titles as t on t.emp_no = e.emp_no
 and t.from_date = s.from_date + interval '2 days'
 ORDER BY e.emp_no
```

USING keyword:
 ```sql
 SELECT emp.emp_no, emp.first_name, dep.dept_no from employee as emp
 INNER JOIN dept as dep USING(emp_no)

 SELECT emp.emp_no, emp.first_name, dep.dept_no, d.dept_name from employees as emp
 INNER JOIN dept_emp as dep using(emp_no)
 INNER JOIN departments as d using(dept_no)
 ```
  

When they got hired and when there title changed?
 ```sql
 SELECT e.emp_no, s.salary, s.from_date, t.title from employees as e
 INNER join salaries as s on e.emp_no = s.emp_no
 INNER join titles as t on t.emp_no = e.emp_no
 AND (t.from_date = s.from_date OR t.from_date = s.from_date + interval '2 days')
 ORDER BY e.emp_no
 ```

##### Always use inner joins for best practices

Self Join: It is a type of inner join but within the same table
  When a table has a foreign key referencing to its own primary key
  ```sql
  SELECT a.id, a.name as emp_name, b.name as supervisor_name from
  employee as a, employee as b
  WHERE a.supervisor_id = b.id
  ```

Outer join: Retrieves those rows also which does not match [LEFT RIGHT FULL]
 ```sql
 SELECT emp.emp_no, dept.emp_no from employees as emp LEFT JOIN dept_manager as dept
 ON emp.emp_no=dept.emp_no
 WHERE dept.emp_no IS NOT NULL;
 ```
  

Less common joins:
  CROSS JOIN: very less common, it is not used in the industry
 ```sql
 SELECT * from tableA CROSS JOIN tableB

 SELECT * from tableA as a CROSS JOIN tableB as b ON a.id = b.id
 ``` 
  FULL JOIN

  

Exercise on Inner Join:
  Get all orders from customers who live in Ohio (OH), New York (NY) or Oregon (OR) state:
  ```sql
  select cust.firstname, cust.lastname, cust.state, ord.orderid, ord.totalamount  from    customers as cust
  INNER JOIN orders as ord on cust.customerid = ord.customerid
  WHERE cust.state IN ('OH', 'NY', 'OR')
  ORDER BY ord.orderid
  ```

  Show me the inventory for each product:
  ```sql
  select prod.prod_id, prod.price, inv.quan_in_stock from products as prod
  INNER JOIN inventory as inv ON prod.prod_id = inv.prod_id
  ORDER BY prod.prod_id
  ```

  Show me for each employee which department they work in:
  ```sql
  select emp.emp_no, dep.dept_no, d.dept_name from employees as emp
  INNER JOIN dept_emp as dep on dep.emp_no = emp.emp_no
  INNER JOIN departments as d ON d.dept_no = dep.dept_no
  order by emp.emp_no
  ```


GROUP BY:
  It splits data into groups or chunks so we can apply functions against the group rather than the entire table
  We use group by almost exclusively with aggregate function
  group by is stricter than it looks
  It reduces all records found for the matching group to a single record
  Group by utilizes split-apply-combine strategy

```sql
SELECT dept_no, count(emp_no) from dept_emp
group by dept_no
```


Having keyword:
  WHERE applies filter to rows
  HAVING applies filter to a group
  ```sql
  SELECT d.dept_name, count(emp.emp_no) as "emp count" from employees as emp
  INNER JOIN dept_emp as dep on dep.emp_no = emp.emp_no
  INNER JOIN departments as d on d.dept_no = dep.dept_no
  GROUP BY d.dept_name
  ```

```sql
SELECT dep.dept_no, d.dept_name, count(emp.emp_no) as "emp count" from employees as emp
INNER JOIN dept_emp as dep on dep.emp_no = emp.emp_no
INNER JOIN departments as d on d.dept_no = dep.dept_no
GROUP BY d.dept_name, dep.dept_no
HAVING count(emp.emp_no) > 25000
```
  



  

  SELECT dep.dept_no, d.dept_name, count(emp.emp_no) as "emp count" from employees as emp

  INNER JOIN dept_emp as dep on dep.emp_no = emp.emp_no

  INNER JOIN departments as d on d.dept_no = dep.dept_no

  WHERE emp.gender = 'F'

  GROUP BY d.dept_name, dep.dept_no

  HAVING count(emp.emp_no) > 25000

  

  SELECT d.dept_name, count(emp.emp_no) as "emp count" from employees as emp

  INNER JOIN dept_emp as dep on dep.emp_no = emp.emp_no

  INNER JOIN departments as d on d.dept_no = dep.dept_no

  GROUP BY d.dept_name, emp.gender

  HAVING count(emp.emp_no) > 25000 and emp.gender = 'F' --same thing as above, dont use this line(after and) if the attrib is not in group by.... the last line works on every group but if it is in where then it works on individual grp

  

ORDER BY in GROUP by clause:

  it always comes after group by

  

  SELECT d.dept_name, count(emp.emp_no) as "emp count" from employees as emp

  INNER JOIN dept_emp as dep on dep.emp_no = emp.emp_no

  INNER JOIN departments as d on d.dept_no = dep.dept_no

  GROUP BY d.dept_name

  ORDER BY d.dept_name desc, count(emp.emp_no)

  
  

Exercises on group by:

  How many people were hired on any given hire date?

    select hire_date, count(emp_no) as "emp_count" from employees

    group by hire_date

    order by emp_count desc

  Show me all the employees, hired after 1991 and count the amount of positions they had:

    select emp.emp_no, count(t.title) as "amount of position" from employees as emp

    inner join titles as t on t.emp_no = emp.emp_no

    where EXTRACT (YEAR FROM hire_date) > 1991

    group by emp.emp_no

  
  

Find the latest salary of employee:

  select emp_no, max(from_date) from salaries

  group by emp_no

  select emp_no, max(from_date), salary from salaries

  group by emp_no, salary -- both queries do not work to get the latest salary of an employee, so we will have to find another way

  

UNION: It combines the two select statements

  The main difference between UNION and UNION ALL is => UNION ALL does not remove duplicate records

  

  SELECT null as prod_id, sum(quantity) from orderlines

  UNION

  SELECT prod_id, sum(quantity) from orderlines

  GROUP BY prod_id

  ORDER BY prod_id DESC

  

  GROUPING SETS - A subclause of group by that allows us to define multiple groupings (as we did with UNION)

  

  SELECT prod_id, sum(quantity) from orderlines     --gives output same as above query

  GROUP BY

    GROUPING SETS(

      (),

      (prod_id)

    )

  ORDER BY prod_id DESC

  

  SELECT prod_id, orderlineid, sum(quantity) from orderlines

  GROUP BY

    GROUPING SETS(

      (),

      (prod_id),

      (orderlineid)

    )

  ORDER BY prod_id DESC

  

Rollup:

  ROLLUP ( a, (b, c), d ) is equivalent to

  GROUPING SETS (

    ( a, b, c, d ),

    ( a, b, c    ),

    ( a          ),

    (            )

  )

  
  

  SELECT EXTRACT (YEAR FROM orderdate), EXTRACT (MONTH FROM orderdate), EXTRACT (DAY FROM orderdate), sum(quantity) FROM orderlines

  GROUP BY

    ROLLUP(

      EXTRACT (YEAR FROM orderdate),

      EXTRACT (MONTH FROM orderdate),

      EXTRACT (DAY FROM orderdate)

    )

  

What have we learned so far:

  1. Grouping data is useful

  2. Grouping happens after from/where

  3. Having is a special filter for groups

  4. Grouping sets and rollup is useful

  -----------------------------------------------------

  

Window functions:

  select d.dept_name, round(avg(salary)) from salaries as sal

  inner join dept_emp as dep on dep.emp_no = sal.emp_no

  inner join departments as d on d.dept_no = dep.dept_no

  group by d.dept_name

  

  Now add the avg to every salary so we could visually see how much each employee is making from the average in each dept

  Window function comes into play

  

  Window functions creates a new column based on functions performed on a subset or "window" of the data

  

  syntax:

    window_function(arg1,arg2,..) OVER (

      [PARTITION BY partition_expression]

      [ORDER BY sort_expression [ASC | DESC] [NULL { FIRST | LAST}]]

    )

  

  select *, max(salary) OVER () from salaries

  select *, max(salary) OVER () from salaries LIMIT 100 -- the max value remains same

  select *, max(salary) OVER () from salaries where salary < 70000 --the max value changes because of the filter

  

  PARTITION BY - divides rows into groups to apply to apply function against (optional)

                  its like a group by statement but used inside window function

  

  select *, avg(salary) OVER () from salaries as sal

  join dept_emp as dep using(emp_no)

  join departments as d using(dept_no)

  

  finding the avg salary of emp of each department:

    select *,

      avg(salary) OVER (

          PARTITION by dep.dept_no -- does the grouping and then find avg of each group

      ) from salaries as sal

    join dept_emp as dep using(emp_no)

    join departments as d using(dept_no)

  

ORDER BY in window function: it acts strange

  select *,

    count(salary) OVER (

        ORDER by emp_no           --only creates one copy of count and adds the count of another count to it just like cummulative sum

    ) as emp_count

  FROM salaries

  order by emp_count

  

FRAME clause:

  when using a window function we can create a subrange or frame

      Key                                       Meaning

  ROWS OR RANGE                         whether you want to use a range or rows as a frame

  PRECEDING                             rows before the current one

  FOLLOWING                             rows after the current one

  UNBOUNDED PRECEDING or FOLLOWING      returns all before or after

  CURRENT ROW                           your current row

  

  eg.

  PARTITION BY category ORDER BY price RANGE BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW

  

  Without order by - by default the framing is usually all partition rows

  

  select *,

    count(salary) OVER (

        PARTITION by emp_no

        order by salary

    )

  FROM salaries

  

  select *,

    count(salary) OVER (

        PARTITION by emp_no

        order by salary

        range BETWEEN UNBOUNDED PRECEDING and UNBOUNDED FOLLOWING

    )

  FROM salaries

  

  select *,

    count(salary) OVER (

        PARTITION by emp_no

        order by salary

        rows BETWEEN UNBOUNDED PRECEDING and UNBOUNDED FOLLOWING

    )

  FROM salaries

  

  -- rows or range does not have any changes effect, both works the same

  

  finding the current salary of an emp:

    SELECT emp.emp_no, emp.first_name, d.dept_name, max(sal.salary)  from salaries as sal

    join employees as emp using(emp_no)

    join dept_emp as dep using(emp_no)

    join departments as d using(dept_no)

    group by emp.emp_no, emp.first_name, d.dept_name

    order by emp_no                                   --the max salary isnt necessary emp makes, there could be demotion too ...wht if emp takes a salary cut

  

    --correct answer

    SELECT DISTINCT emp.emp_no, emp.first_name, d.dept_name,

    LAST_VALUE(sal.salary) OVER (

        PARTITION by emp.emp_no

        order by sal.from_date

        range BETWEEN UNBOUNDED PRECEDING and UNBOUNDED following

    )

    from salaries as sal

    join employees as emp using(emp_no)

    join dept_emp as dep using(emp_no)

    join departments as d using(dept_no)

    order by emp.emp_no

  

FIRST_VALUE: an aggregate func which returns a value evaluated against the first row within its partition

  I want to know how my price compares to the item with the lowest price in the same category

    2 ways -

    select p.prod_id, p.price, p.category, c.categoryname,

    first_value(price) over(

        PARTITION by p.category

        order by price

        range BETWEEN UNBOUNDED PRECEDING and UNBOUNDED FOLLOWING

    )

    from products as p

    join categories as c USING(category);

  

    select p.prod_id, p.price, p.category, c.categoryname,

    min(price) over(

        PARTITION by p.category

    )

  from products as p

  join categories as c USING(category);

  

LAST_VALUE: an aggregate func which returns a value evaluated against the last row within its partition

  I want to know how my price compares to the item with the highest price in the same category

    select p.prod_id, p.price, p.category, c.categoryname,

    last_value(price) over(                 -- we can use max like we did in prev one

        PARTITION by p.category

        order by price

        range BETWEEN UNBOUNDED PRECEDING and UNBOUNDED FOLLOWING

    )

    from products as p

    join categories as c USING(category);

  

How much amount a particular has spent over orders:

  select customerid, orderid, totalamount,

    sum(totalamount) over(

        PARTITION by customerid

        order by orderid

    ) as "cum sum"

  from orders

  

Row number: Number the current row within the partition starting from 1

  I want to know where my product is positioned in the category by price

    select p.prod_id, p.category, c.categoryname,

    row_number() over(

        PARTITION by category

        order by price

    ) as "position in category"

    from products as p

    join categories as c using(category)

  

Exercise on window function:

  Show the population per continent

    select distinct continent,

    sum(population) over (

        PARTITION by continent

    ) as "content population"

    from country

  

  To the previous query add on the ability to calculate the percentage of the world population

  What that means is that you will divide the population of that continent by the total population and multiply by 100 to get a percentage.

  Make sure you convert the population numbers to float using `population::float` otherwise you may see zero pop up:

    select distinct continent,

    sum(population) over (

        PARTITION by continent

    ) as "content population",

    concat(

        round(

            (sum(population::float4) over(PARTITION by continent) / sum(population::float4) over ()) * 100

        ), '%'

    ) as "percentage of population"    

    from country

  

  Count the number of towns per region:

    select distinct r.id, "r"."name",

    count(t.id) over(

        PARTITION by r.id

        order by "r"."name"

    )

    from regions as r

    join departments as d on d.region = r.code

    join towns as t on t.department = d.code

  

Case statements:

  SELECT a,

      Case

        When a=1 then "first"

        When a=2 then "second"

        else "other"

      END from test

  

  SELECT orderid, customerid,

      CASE

        When customerid = 1 then "first customer"

        else "not first customer"

      END, orderamount from orders

  

  SELECT orderid, customerid, netamount from orders

    WHERE CASE WHEN customerid > 10 THEN netamount < 100 ELSE netamount > 100 END

    ORDER BY customerid

NULLIF:

  NULLIF(val1, val2)  --if val1 equals val2 return null

  NULLIF(0,0)         -- NULL

  NULLIF('ABC','DEF') -- ABC

  

  Fill in empty spots with a null to avoid divide by zero issues

Views:

  It allows us to store and query previously run queries

  There are 2 types of views - materialized and non materialized

  

  non materialized - query gets re-run each time the view is called

  materialized - stores the data physically and periodically updates it when tables change

  

  Creating a view: CREATE VIEW view_name As query

  

  Views are the output of the query we ran

  View take very little space to store, we only store the definition of a view not all the data that it returns (not materialized)

  

  Updating a view: CREATE OR REPLACE view_name AS query

  Rename a view: ALTER VIEW view_name RENAME TO new_name

  Deleting a view: DROP VIEW IF EXISTS view_name

                   DROP VIEW view_name

  

  CREATE VIEW last_salary_change AS

    select emp.emp_no, max(sal.from_date)

    from employees as emp

    join salaries as sal using(emp_no)

    group by emp.emp_no;

  SELECT * from last_salary_change

  

  select * from salaries as sal

  join last_salary_change as l using(emp_no)

  where sal.from_date = l.max

  

  finding the current salary of employee: (2nd way)

    select l.emp_no, l.max, s.from_date, d.dept_name, s.salary

    from last_salary_change as l

    join salaries as s using(emp_no)

    join dept_emp as dep using(emp_no)

    join departments as d using(dept_no)

    where l.max = s.from_date

  

Index:

  It is a construct to improve querying performance

  It is a pointer to data in a table

  Think of it like a table of contents and it helps to find where a piece of data is

  What does it do?

    Speeds up the query

    slows down data insertion and updates

  Types of indexes:

    single-column, multi-column, unique, partial, implicit indexes

  

  Creating an index:

    CREATE UNIQUE INDEX name on table_name (col1, col2,...);

  Deleting an index:

    DROP INDEX name;

  

  when to use index:

    index foreign keys

    index primary keys and unique columns

    index on columns that end up in the order by/where clause often

  

  when not to use:

    dont add index just to add an index

    dont use index on small tables

    dont use on tables that are updated frequently

    dont use on columns that contain null values

    dont use on columns that have large values (description, etc)

  

  when to use

    single-column - when there is only one condition in a where clause

    multi-column - when there is only more than condition in a where clause

    unique - for speed and integrity

    partial - index over a subset of a table

              CREATE INDEX <name> on <table> (<expression>)

    implicit - automatically created by the db i.e primary key, unique key

  

  step 1:

    EXPLAIN ANALYZE

    select * from city

    where countrycode in ('TUN', 'BE', 'NL')

  

  step 2:

    CREATE INDEX idx_countrycode ON city (countrycode)

  

  step 3:

    EXPLAIN  ANALYZE

    select * from city

    where countrycode in ('TUN', 'BE', 'NL')

  It speed up our query

  

    CREATE INDEX idx_countrycode ON city (countrycode) where countrycode IN ('TUN', 'BE', 'NL') => partial index

  

Index algorithms:

  B-tree, Hash, Gin, Gist

  Each index type uses a different algorithm

  

  CREATE UNIQUE INDEX name ON table_name USING method (col1, col2,...)

  

  CREATE INDEX idx_countrycode ON city using HASH (countrycode)

  

  B-tree:

    default algorithm

    best used for comparisons with <, <=, =, >=, BETWEEN, IN, IS NULL, IS NOT NULL

  Hash:

    can only handle equality operations

  GIN: Generalized Inverted Index

    Best used when multiple values are stored in single field

    eg. Array

  GIST: Generalized Search Tree

    useful in indexing geometric data and full text search

  

Subquery:

  A construct that allows us to build extremely complex queries

    also called an inner query or inner select

  Its a query within another SQL query most often found in the where clause

  

  SELECT * FROM <table> WHERE <column> <condition> (

    SELECT <col1>, <col2>,... FROM <table> [WHERE | GROUP BY | ORDER BY]

  )

  

  We can also use it in the select, from and having clause

  

  select (

    select <col1>, <col2>, .. from <table> [WHERE | GROUP BY | ORDER BY]

  ) from <table> as <name>

  

  select * from (

    select <col1>, <col2>, .. from <table> [WHERE | GROUP BY | ORDER BY]

  ) as <name>

  

  select * from <table> as <name> HAVING (

    select <col1>, <col2>,.. from <table> [WHERE | GROUP BY | ORDER BY]

  )

  

          Subquery                                   JOIN

  Subqueries are query that could         Joins combine rows from one or more tables based on a

  stand alone                             match condition

  Subqueries return a single result       joins can only return a row set

  or a row set

  

  select title, price, (select avg(price) from products) as "global avg"

  from products

  

  select title, price, (select avg(price) from products) as "global avg"

  from (

    select * from products

  ) as "products sub"

  

  select title, price, (select avg(price) from products) as "global avg"

  from (

    select * from products where price < 10

  ) as "products sub"               -- global avg remains same but table only shows product with price<10

  

  Guidelines:

    Subquery must be enclosed in the parenthesis

    Put subquery on the RHS

    Order by in a subquery, it gets ignored

    Subqueries that return null may not return results

  

  Types of subquery:

    Single row

    Multiple row

    Multiple column

    Corelated

    Nested

  

    Single row:

      select name, salary from salaries where salary >= (select avg(salary) from salaries)

  

      select name, salary, (

        select avg(salary) from salaries

      ) as "avg sal"

      from salaries

  

    Multiple row: returns one or more rows

      select title, price, category from products

      where category in (

        select category from categories where categoryname in ('Comedy', 'Family', 'Classics')

      )

    Multiple column: returns one or more columns

      select s.emp_no, s.salary, dea.avg as "Dept avg sal"

      from salaries as s

      join dep_emp as de using(emp_no)

      join (

        select dept_no, avg(salary) from salaries as s2

        join dept_emp as e using(emp_no)

        group by dept_no

      ) as dea using(dept_no)

      where salary > dea.avg

  

DBMS:

  DDL: Commands to create, modify, delete different structures

        structures => databases, tables, roles

  DML: Commands for adding, deleting, modifying data

  

  Types of dbs:

    Regular and template db

  Default postgres db:

    this is the default db that is created when we setup postgres (initdb)

      psql -U <user> <database>

    postgres by default will assume a connection to a db with the same name as the user if no db is supplied

      psql -U postgres

    Always use the db name

  Template db:

    Template db is used to be a blueprint for regular dbs

      Template0 is made for template1 and so on ... Template0 => Template1

      Never change Template0

      Any change we make to the template1 are automatically applied to all new dbs

  

    CREATE DATABASE superdupertemplate;

    CREATE TABLE superdupertable();

    CREATE DATABASE superduperdatabase WITH TEMPLATE superdupertemplate;

  Creating a database:

    CREATE DATABASE <name>

    [ [WITH]

    [OWNER  user_name]

    [TEMPLATE template]

    [ENCODING encoding]

    [LC_COLLATE lc_collate]

    [LC_CTYPE lc_ctype]

    [TABLESPACE tablespace]

    [CONNECTION LIMIT connlimit] ]

  

    defaults:

      Template => template01

      encoding => UTF8

      CONNECTION_LIMIT => 100

      Owner => Current User

  

  Database Organisation:

    postgres offers the concept of "Schemas"

    think of it like a box in which we can organize tables, views, indexes, etc.

      It creates a public schema...

      select * from emp is same as select * from public.emp

      \dn : show all schemas

  

      CREATE SCHEMA sales

    Reasons to use schemas:

      to allow user to use one db without interfering with each other

      to organise db objects into logical groups to make them more manageable

  

    Creating db is a restricted action, not everyone is allowed to do it

  

  Roles in postgres:

    roles are vital to any dbms they determine whats allowed

    A role can be a user or a group, it depends on how you setup the role

    Roles have the ability to grant membership to another role

  

    Roles has attribute and privilege

    attribute defines privilege like superuser

    When a role is created it is given certain attributes

  

    Role => createdb/nocreatedb superuser/nosuperuser createrole/nocreaterole login/nologin

    By default only the creator of the db or superuser has access to all its objects

  

    Creating a role:

      CREATE ROLE readonly WITH LOGIN ENCRYPTED PASSWORD 'readonly';

      CREATE ROLE employee_read;

    Creating a user:

      CREATE USER user1 WITH ENCRYPTED PASSWORD 'user1'; -- Role and user is the same thing, only difference is that the user assumes LOGIN by default

  

    creatuser --interactive (commandline)

    createuser -U postgres --interactive

  

    Alter role:

      Alter role testuser WITH ENCRYPTED PASSWORD 'password';

  

    show hba_file; --it has all the authentication conf

    show config_file; --config file of postgres contains everything

  

  Granting Privileges:

    GRANT ALL PRIVILEGES ON <table> TO <user>;

    GRANT ALL ON ALL TABLES [IN SCHEMA <schema>] TO <user>;

    GRANT [SELECT, UPDATE, INSERT, ...] ON <table> [IN SCHEMA <schema>] TO <user>

  Examples:

    GRANT SELECT ON titles TO privtest;

    REVOKE SELECT ON titles FROM privtest;

  

    GRANT ALL ON ALL TABLES IN SCHEMA public TO privtest;

    REVOKE ALL ON ALL TABLES IN SCHEMA public FROM privtest;

  

    CREATE ROLE employee_read

    GRANT SELECT ON ALL TABLES IN SCHEMA public TO employee_read

    GRANT employee_read TO privtest;

    REVOKE employee_read FROM privtest;

  Best practice for role management:

    when managing roles and permissions, always go with the principle of least privilege

    Give specific privilege to user

    Dont user superuser/admin privilege by default