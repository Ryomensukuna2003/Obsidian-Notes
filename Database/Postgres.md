### Flow of query

**FROM -> WHERE -> GROUP BY -> HAVING -> DISTINCT -> SELECT -> ORDER BY -> LIMIT**

**SELECT**
- Use the `SELECT ... FROM` statement to retrieve data from a table.
- Use a column alias to assign a temporary name to a column or an expression in a query.
-  Clauses used in SELECT
	- `DISTINCT` → unique rows
	- `ORDER BY`→ sort
	- `WHERE` → filter
	- `LIMIT` / `FETCH`→ limit rows
	- `GROUP BY` → group rows
	- `HAVING`→ filter groups
	- `JOINS`: `INNER`, `LEFT`, `FULL`, `CROSS`
	- `SETS`: `UNION`, `INTERSECT`, `EXCEPT`

- Using PostgreSQL SELECT statement with expressions example
```sql
--Return full names from column first_name and last_name 
SELECT first_name || ' ' || last_name FROM customer;
```


**ORDER BY**
- Sort with `ASC` (default) or `DESC`
- Use `NULLS FIRST` / `NULLS LAST` for null ordering
```sql
SELECT first_name, last_name FROM customer ORDER BY last_name DESC;
SELECT first_name, LENGTH(first_name) len FROM customer ORDER BY len DESC;
SELECT num FROM sort_demo ORDER BY num NULLS LAST;
```

**DISTINCT**
- Removes duplicate rows
```sql
SELECT DISTINCT column FROM table;
```

**WHERE**
- Filters rows based on condition

| Operator | Description        |
| -------- | ------------------ |
| =, >, <  | Comparison         |
| >=, <=   | Range comparison   |
| != or <> | Not equal          |
| AND, OR  | Logical operators  |
| IN       | Match any in list  |
| BETWEEN  | Match within range |
| LIKE     | Pattern match      |
| IS NULL  | Check for NULL     |
| NOT      | Negates condition  |
| ::       | Cast operator      |
Example :
```sql
SELECT first_name, last_name FROM customer
WHERE first_name IN ('Ann', 'Anne', 'Annie');
```

**LIMIT and OFFSET**
If you want to skip a number of rows before returning the `row_count` rows, you can use `OFFSET` clause placed after the `LIMIT` clause:
``` sql
SELECT film_id, title, release_yea FROM film
ORDER BY film_id
LIMIT 4 OFFSET 3;
```

**Fetch**
Use `FETCH` clause to skip a certain number of rows and retrieve a specific number of rows from a query.
```sql
-- OFFSET row_to_skip { ROW | ROWS } FETCH { FIRST | NEXT } [ row_count ] { ROW | ROWS } ONLY

SELECT film_id, title FROM film
ORDER BY title
OFFSET 5 ROWS
FETCH FIRST 5 ROW ONLY;
```

**Like**
- `%` → match any chars
- `_` → match single char
- `ESCAPE` → define escape char
- `NOT LIKE` → negation
- `ILIKE` → case-insensitive match

What if we want to fetch data LIKE 10% as % is wildcard?
```sql
SELECT * FROM t
WHERE message LIKE '%10$%%' ESCAPE '$';
```

## PostgreSQL Joins

**Inner JOIN** - Take common data from both tables and leave rest
![[Pasted image 20250415114031.png]]

**Left JOIN** - Take all data from left table and only common data from Right table
![[Pasted image 20250415114133.png]]

**Right JOIN** - Take all data from Right table and only common data from Left table
![[Pasted image 20250415114245.png]]

**Full Outer JOIN** - Take all data from both tables and fill NULL if there is no match
![[Pasted image 20250415114401.png]]

**Self JOIN** - A self-join is a regular join that joins a table to itself.
```sql
employee_id | first_name |  last_name  | manager_id
-------------+------------+-------------+------------
           1 | Windy      | Hays        |       null
           2 | Ava        | Christensen |          1
           3 | Hassan     | Conner      |          1
           4 | Anna       | Reeves      |          2
           5 | Sau        | Norman      |          2
           6 | Kelsie     | Hays        |          3
           7 | Tory       | Goff        |          3
           8 | Salley     | Lester      |          3
(8 rows)
```
List all employee with their manager names
```sql
SELECT
  e.first_name || ' ' || e.last_name employee,
  m.first_name || ' ' || m.last_name manager
FROM
  employee e
  INNER JOIN employee m ON m.employee_id = e.manager_id
ORDER BY
  manager;
```
Output:
```sql
employee         | manager
-----------------+-----------------
 Sau Norman      | Ava Christensen
 Anna Reeves     | Ava Christensen
 Salley Lester   | Hassan Conner
 Kelsie Hays     | Hassan Conner
 Tory Goff       | Hassan Conner
 Ava Christensen | Windy Hays
 Hassan Conner   | Windy Hays
(7 rows)
```
Here same table is used twice with Alias 

**Cross JOIN** - Combine each row of first table to each row with second table resulting every possible combination exits
![[Pasted image 20250415125809.png]]


**Natural JOIN** - Automatically joins two tables using columns with the same names and compatible data types. [If no common columns: returns Cartesian product.]
The `products` table has the following data:

```sql
product_id |  product_name   | category_id
------------+-----------------+-------------
          1 | iPhone          |           1
          2 | Samsung Galaxy  |           1
          3 | HP Elite        |           2
          4 | Lenovo Thinkpad |           2
          5 | iPad            |           3
          6 | Kindle Fire     |           3
(6 rows)
```

The `categories` table has the following data:

```sql
category_id | category_name
-------------+---------------
           1 | Smartphone
           2 | Laptop
           3 | Tablet
           4 | VR
(4 rows)
```

```sql
SELECT * FROM products
NATURAL JOIN categories;
```

Output:
```sql
category_id | product_id |  product_name   | category_name
-------------+------------+-----------------+---------------
           1 |          1 | iPhone          | Smartphone
           1 |          2 | Samsung Galaxy  | Smartphone
           2 |          3 | HP Elite        | Laptop
           2 |          4 | Lenovo Thinkpad | Laptop
           3 |          5 | iPad            | Tablet
           3 |          6 | Kindle Fire     | Tablet
(6 rows)
```


## GROUPING

**GROUP BY**
	GROUP BY divides rows and for each group we can return aggregate function such as SUM() or COUNT()

```sql
SELECT customer_id, staff_id, SUM(amount)
FROM payment
GROUP BY
  staff_id,
  customer_id;
```

**HAVING vs. WHERE**
	The `WHERE` clause filters the rows based on a specified condition whereas the `HAVING` clause filter groups of rows according to a specified condition.

```sql
SELECT store_id, COUNT (customer_id)
FROM customer
GROUP BY store_id
HAVING COUNT (customer_id) > 300;
```

**GROUPING SET**
	What if we have to join multiple GROUP BY clauses?
	We can use UNION ALL statement like below
```sql
SELECT brand, segment, SUM (quantity FROM sales
GROUP BY brand, segment

UNION ALL

SELECT brand, segment, SUM (quantity FROM sales
GROUP BY brand

UNION ALL

SELECT NULL, NULL, SUM (quantity)
FROM sales;
```

Issues?
- First, it is quite lengthy.
- Second, it has a performance issue because PostgreSQL has to scan the `sales` table separately for each query.

```sql
SELECT brand, segment, SUM (quantity)
FROM sales
GROUP BY
    GROUPING SETS (
        (brand, segment),
        (brand),
        (segment),
        ()
    );
```

**CUBE**
`CUBE (c1,c2,c3)` makes all eight possible grouping sets (2^n):
```sql
(c1, c2, c3)
(c1, c2)
(c2, c3)
(c1,c3)
(c1)
(c2)
(c3)
()
```

**ROLLUP**
`ROLLUP(c1,c2,c3)` generates only four grouping sets, assuming the hierarchy `c1 > c2 > c3` as follows:
```sql
(c1, c2, c3)
(c1, c2)
(c1)
()
```

Example
```sql
SELECT brand, segment, SUM (quantity) FROM sales
GROUP BY
    ROLLUP (brand, segment);
```



## Subquery

```sql
SELECT city FROM city
WHERE country_id = (
    SELECT country_id FROM country
    WHERE country = 'United States'
  );
```

**Correlated Subquery**
```sql
SELECT film_id, title, length, rating FROM film f
WHERE length > (
    SELECT AVG(length)
    FROM film
    WHERE rating = f.rating -- Using outer query 
);
```

**ANY/SOME Operator** - Used to compare or use operators like =, <, >, <=, >=, and <> on Inner or outer queries

List employees who have the salary the same as manager:
```sql
SELECT * FROM employees
WHERE salary = ANY (
    SELECT salary FROM managers
	);
```

**ALL operator** - ALL operator is used to compare a value with all values in a set of values returned by a subquery.
```sql
SELECT * FROM employees
WHERE salary > ALL(
    SELECT salary FROM managers
  );
```

**Exists** - Used to check the existence of rows in a subquery.
```sql
SELECT first_name, last_name
FROM customer
WHERE EXISTS( SELECT NULL );
```


## Common Table expression (CTE)
- Use a common table expression (CTE) to create a temporary result set within a query.
- Leverage CTEs to simplify complex queries and make them more readable.
```sql
WITH action_films AS (
  SELECT
    f.title,
    f.length
  FROM
    film f
    INNER JOIN film_category fc USING (film_id)
    INNER JOIN category c USING(category_id)
  WHERE
    c.name = 'Action'
)
SELECT * FROM action_films;
```

## Recursive Query

```cardlink
url: https://neon.tech/postgresql/postgresql-tutorial/postgresql-recursive-query
title: "PostgreSQL Recursive Query"
description: "In this tutorial, you will learn about the PostgreSQL recursive query using the recursive common table expressions (CTEs)."
host: neon.tech
favicon: https://neon.tech/favicon/favicon.png
image: https://neon.tech/docs/og?title=UG9zdGdyZVNRTCBSZWN1cnNpdmUgUXVlcnk=
```

- Use the `WITH RECURSIVE` syntax to define a recursive query.
- Use a recursive query to deal with hierarchical or nested data structures such as trees or graphs.


## Modifying Data

**INSERT** 
- Use PostgreSQL `INSERT` statement to insert a new row into a table.
- Use the `RETURNING` clause to get the inserted rows.

```sql
INSERT INTO table1(column1, column2, …)
VALUES (value1, value2, …)
RETURNING *;

-- Multiple rows

INSERT INTO contacts (first_name, last_name, email)
VALUES
    ('John', 'Doe', 'john.doe@example.com'),
    ('Jane', 'Smith', 'jane.smith@example.com'),
    ('Bob', 'Johnson', 'bob.johnson@example.com');
```

**CREATE** 
- Specify a condition in a WHERE clause to determine which rows to update data.
- Use the `RETURNING` clause to return the updated rows from the `UPDATE` statement
```sql
UPDATE courses
SET published_date = '2020-08-01'
WHERE course_id = 3;

-- Updating with refrence with 2nd table
UPDATE product
SET net_price = price - price * discount
FROM product_segment
WHERE product.segment_id = product_segment.id;

```

**DELETE**
- Use the `DELETE FROM` statement to delete one or more rows from a table.
- Use the `WHERE` clause to specify which rows to be deleted.
- Use the `RETURNING` clause to return the deleted rows.

**UPSERT** (Upsert is a combination of update and insert)
- First, the `ON CONFLICT` clause identifies the conflict target which is usually a unique constraint (or a unique index). If data violates the constraint, a conflict occurs.
- Second, the `DO UPDATE` update existing rows or do nothing rather than aborting the entire operation when a conflict occurs.
- Third, the `SET` clause defines the columns and values to update. The `EXCLUDED` allows you to access the new values.
```sql
INSERT INTO inventory (id, name, price, quantity)
VALUES (1, 'A', 16.99, 120)
ON CONFLICT(id)
DO UPDATE SET
  price = EXCLUDED.price,
  quantity = EXCLUDED.quantity;
```

**MERGE**
- The `MERGE` statement allows you to conditionally `INSERT`, `UPDATE`, or `DELETE` rows based on matching records in a source table.
```sql
MERGE INTO products p
USING product_updates u
ON p.name = u.name
WHEN MATCHED AND u.status = 'discontinued' THEN
    DELETE
WHEN MATCHED AND u.status = 'active' THEN
    UPDATE SET
        price = COALESCE(u.price, p.price),
        stock = u.stock,
        status = u.status,
        last_updated = CURRENT_TIMESTAMP
WHEN NOT MATCHED AND u.status = 'active' THEN
    INSERT (name, price, stock, status)
    VALUES (u.name, u.price, u.stock, u.status)
RETURNING
    merge_action() as action,
    p.product_id,
    p.name,
    p.price,
    p.stock,
    p.status,
    p.last_updated;
```

## Set Operations

**UNION**  
- Used to merge 2 tables with same columns
- Use the `UNION` to return distinct rows.
- Use the `UNION ALL` to retain the duplicate rows.
![[Pasted image 20250416154858.png]]

```sql
SELECT * FROM top_rated_films
UNION ALL
SELECT * FROM most_popular_films
```

**INTERSECT**
- Get common part from both tables
![[Pasted image 20250416154842.png]]

```sql
SELECT * FROM most_popular_films
INTERSECT
SELECT * FROM top_rated_films;
```

**EXCEPT**
- The `EXCEPT` operator returns distinct rows from the first (left) query that are not in the second (right) query.
 ![[Pasted image 20250416154805.png]]
```sql
SELECT * FROM top_rated_films
EXCEPT
SELECT * FROM most_popular_films;
```

## Transaction

A database transaction is a single unit of work that consists of one or more operations.

A PostgreSQL transaction is atomic, consistent, isolated, and durable [ACID]:

- **Atomicity** is either fully complete or fail
- **Consistency** ensures that changes to data written to the database are valid and adhere to predefined rules.
- **Isolation** determines how the integrity of a transaction is visible to other transactions.
- **Durability** ensures that transactions that have been committed are permanently stored in the database.

When we execute a statement, PostgreSQL implicitly wraps it in a transaction.

**Start a transaction**
To start a transaction explicitly, you execute either one of the following statements:
```sql
BEGIN TRANSACTION;
-- OR
BEGIN WORK;
-- OR
BEGIN;
```

**Commit a transaction**
To permanently apply the change to the database, you commit the transaction by using the `COMMIT WORK` statement:
```postgresql
COMMIT WORK;
-- OR
COMMIT TRANSACTION;
-- OR
COMMIT
```

Put it all together.

```postgresql
-- start a transaction
BEGIN;

-- insert a new row into the accounts table
INSERT INTO accounts(name,balance)
VALUES('Alice',10000);

-- commit the change (or roll it back later)
COMMIT;
```

**Roll back a transaction**
If you want to undo the changes to the database, you can use the ROLLBACK statement:

```postgresql
ROLLBACK;
-- OR
ROLLBACK TRANSACTION;
-- OR 
ROLLBACK WORK;
```

## Data Types

PostgreSQL supports the following data types:
- **Boolean** 
	- (1, yes, y, t, true)
	- (0, no, n, f, false)

- Character = **char**, **varchar** , and **text**. 
	- `CHAR(n)` is the fixed-length character with space padded. If inserted string is shorter than length of column, PostgreSQL pads spaces else PostgreSQL will throw an error.
	- `VARCHAR(n)` is the variable-length character string. PostgreSQL does not pad spaces when the stored string is shorter than the length
	- `TEXT` is the variable-length character string. Theoretically, text data is a character string with unlimited length.

- Numeric = **integer** and **floating-point number**. 
	- Integer
		- Small integer ( `SMALLINT`) is a 2-byte signed integer that has a range from -32,768 to 32,767.
		- Integer ( `INT`) is a 4-byte integer that has a range from -2,147,483,648 to 2,147,483,647.
		- Serial is the same as integer except that PostgreSQL will automatically generate and populate values into the `SERIAL` column.
	- Floating-point number
		- `float(n)`  is a floating-point number whose precision, is at least, n, up to a maximum of 8 bytes.
		- `real`or `float8`is a 4-byte floating-point number.
		- `numeric`or `numeric(p,s)` is a real number with p digits with s number after the decimal point. This `numeric(p,s)` is the exact number.

- Temporal = **date**, **time**, **timestamp**, and **interval** 
	- `DATE` stores the dates only.
	- `TIME` stores the time of day values.
	- `TIMESTAMP` stores both date and time values.
	- `TIMESTAMPTZ` is a timezone-aware timestamp data type. It stores according to current timezone
	- `INTERVAL` stores periods.

- **UUID** for storing Universally Unique Identifiers 
	- The `UUID` values guarantee a better uniqueness than `SERIAL` and can be used to hide sensitive data exposed to the public such as values of `id` in URL.

- **Array**
```postgresql
CREATE TABLE contacts (
  phones TEXT []
);
```

- **JSON**
	- The `JSON` data type stores plain JSON data that requires reparsing for each processing, while `JSONB` data type stores `JSON` data in a binary format which is faster to process but slower to insert. In addition, `JSONB` supports indexing, which can be an advantage.
```postgresql
INSERT INTO products(name, properties)
VALUES('Ink Fusion T-Shirt','{"color": "white", "size": ["S","M","L","XL"]}')
RETURNING *;
```

- **hstore** stores key-value pair
```postgresql
INSERT INTO books (title, attr)
VALUES
  (
    'PostgreSQL Tutorial',
    '"paperback" => "243",
     "publisher" => "postgresqltutorial.com",
     "language"  => "English",
     "ISBN-13"   => "978-1449370000",
     "weight"    => "11.2 ounces"'
  );
```

- Besides the primitive data types, PostgreSQL also provides several special data types related to geometry and network.
	- `box` – a rectangular box.
	- `line`– a set of points.
	- `point` – a geometric pair of numbers.
	- `lseg` – a line segment.
	- `polygon` – a closed geometric.
	- `inet` – an IP4 address.
	- `macaddr`– a MAC address.


## Managing tables

**CREATE TABLE**
```postgresql
CREATE TABLE [IF NOT EXISTS] table_name (
   column1 datatype(length) column_constraint,
   column2 datatype(length) column_constraint,
   ...
   table_constraints
);
```

- Constraints
	- NOT NULL
	- UNIQUE
	- PRIMARY KEY - Unique identifier for a row
	- CHECK
	- FOREIGN KEY - Column of another table


**CREATE TABLE AS**
Used to create a new table from the result of a query.
```postgresql
CREATE TABLE action_film
AS
SELECT film_id, title, release_year,
FROM fil INNER JOIN film_category USING (film_id)
WHERE category_id = 1;
```

**SERIAL** - Use the PostgreSQL pseudo-type `SERIAL` to create an auto-increment column for a table.

**SEQUENCE** - A database object that allows you to generate a sequence of unique integers

```sql
CREATE SEQUENCE mysequence
INCREMENT 5
START 100;
```

To get the next value from the sequence, you use the `nextval()` function:

```postgresql
SELECT nextval('mysequence');
```
![PostgreSQL Sequence - simple example](https://neon.tech/_next/image?url=%2Fpostgresqltutorial%2FPostgreSQL-Sequence-simple-example.png&w=640&q=75&dpl=dpl_6mdXUz1Mk8U1dkngngPC9J7ipbHb)
```postgresql
SELECT nextval('mysequence');
```
![PostgreSQL Sequence - nextval example](https://neon.tech/_next/image?url=%2Fpostgresqltutorial%2FPostgreSQL-Sequence-nextval-example.png&w=640&q=75&dpl=dpl_6mdXUz1Mk8U1dkngngPC9J7ipbHb)


- OPTIONS
```postgresql
CREATE SEQUENCE [ IF NOT EXISTS ] sequence_name
    [ AS { SMALLINT | INT | BIGINT } ] -- datatype
    [ INCREMENT [ BY ] increment ] -- how much increment
    [ MINVALUE minvalue | NO MINVALUE ] -- min val
    [ MAXVALUE maxvalue | NO MAXVALUE ] --max val
    [ START [ WITH ] start ] -- starting number
    [ CACHE cache ] -- how many number should preallocated for faster access
    [ [ NO ] CYCLE ] -- Should i restart if max val is reached 
    [ OWNED BY { table_name.column_name | NONE } ] -- which column should i associate with
```

**Identity Column**
- This will generate a number for color_name
- If ALWAYS is specified in create query then explicitly defining color_id will result in error
- If DEFAULT is used then no error will be thrown on explicit defining of color_id
```postgresql
CREATE TABLE color (
    color_id INT GENERATED ALWAYS AS IDENTITY,
    color_name VARCHAR NOT NULL
);
```

- Options can be used in Identity column 
```postgresql
CREATE TABLE color (
    color_id INT GENERATED BY DEFAULT AS IDENTITY
    (START WITH 10 INCREMENT BY 10),
    color_name VARCHAR NOT NULL
);
```

**Generated Columns**
Generated column is a column whose values are automatically calculated based on expressions or values from other columns.
- Stored: Calculated when data is inserted or updated and occupies storage space.
- Virtual: Computed when it is read and does not occupy storage space.

```postgresql
CREATE TABLE contacts(
   id SERIAL PRIMARY KEY,
   first_name VARCHAR(50) NOT NULL,
   last_name VARCHAR(50) NOT NULL,
   full_name VARCHAR(101) GENERATED ALWAYS AS (first_name || ' ' || last_name) STORED,
   email VARCHAR(300) UNIQUE
);
```


**ALTER TABLE**
- Add a col
- Drop a col
- Change datatype
- Rename a col
- Set a default val
- Add constraint
- Rename a table

```postgresql
ALTER TABLE links 

--To drop a col
DROP COLUMN column_name;

--renaming a col
RENAME COLUMN column_name
TO new_column_name;

--Chnage default
ALTER COLUMN column_name
[SET DEFAULT value | DROP DEFAULT];
```


**DROP TABLE**
- Use the `CASCADE` option to remove the table and its dependent objects.
- Use the `RESTRICT` option rejects the removal if there is any object depending on the table. The `RESTRICT` option is the default.
```postgresql
DROP TABLE [IF EXISTS] table_name
[CASCADE | RESTRICT];
```

**TRUNCATE TABLE**
The `TRUNCATE TABLE` statement deletes all data from a table very fast. Here’s the basic syntax of the `TRUNCATE TABLE` statement:

```postgresql
TRUNCATE TABLE table_name;
```

By default, the `TRUNCATE TABLE` statement does not remove any data that has foreign key references.

To remove data from a table that have foreign key references, you use `CASCADE` option in the `TRUNCATE TABLE` statement
```postgresql
TRUNCATE TABLE table_name CASCADE;
```



## Database Constraints

|Constraint|Description|
|---|---|
|**Primary Key**|Column or a group of columns used to uniquely identify a row in a table.|
|**Foreign Key**|Column or a group of columns that uniquely identifies a row in another table.|
|**Check**|Ensures that values in a column or a group of columns meet a specific condition.|
|**UNIQUE**|Every time you insert or update a row, it checks if the value is already in the table.|
|**NOT NULL**|Enforce a column to not accept NULL.|
|**DEFAULT**|Define a default value for a table column.|
|**DELETE CASCADE**|Referential action that allows you to automatically delete related rows in child tables when a parent row is deleted from the parent table.|

Example
```postgresql
CREATE TABLE departments (
    dept_id SERIAL PRIMARY KEY,
    dept_name VARCHAR(100) UNIQUE NOT NULL
);

CREATE TABLE employees (
    emp_id SERIAL PRIMARY KEY,
    emp_name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    age INT CHECK (age >= 18 AND age <= 65),
    dept_id INT REFERENCES departments(dept_id) ON DELETE CASCADE,
    salary NUMERIC(10, 2) DEFAULT 30000.00 NOT NULL
);
```


## Conditional
**CASE**
- CASE expression is the same as IF/ELSE
```postgresql
SELECT
  SUM (
    CASE WHEN rental_rate = 0.99 THEN 1 ELSE 0 END
  ) AS "Economy",
  SUM (
    CASE WHEN rental_rate = 2.99 THEN 1 ELSE 0 END
  ) AS "Mass",
  SUM (
    CASE WHEN rental_rate = 4.99 THEN 1 ELSE 0 END
  ) AS "Premium"
FROM
  film;
```

**COALESCE**
- The COALESCE() function accepts multiple arguments and returns the first argument that is not null. If all arguments are null, the COALESCE() function will return null.
- Use the `COALESCE()` function to substitute null values in the query.

**ISNULL**
```postgresql
ISNULL(expression, replacement)
```

**NULLIF**
- The `NULLIF` function returns a null value if argument_1 equals to argument_2, otherwise, it returns argument_1.
```postgresql
SELECT NULLIF (1, 1); -- return NULL
```

**CAST**

```postgresql
SELECT
  CAST ('100' AS INTEGER);
```



## Aggregate Functions

- AVG() – return the average value.
- COUNT() – return the number of values.
- MAX() - return the maximum value.
- MIN() – return the minimum value.
- SUM() – return the sum of all or distinct values.
```postgresql
SELECT column1, AGGREGATE_FUNCTION(column2)
FROM table1
GROUP BY column1;
```

In this syntax, the `GROUP BY` clause divides the result set into groups of rows and the aggregate function performs a calculation on each group e.g., maximum, minimum, average, etc.


- ARRAY_AGG() function
	The PostgreSQL `ARRAY_AGG()` function is an aggregate function that accepts a set of values and returns an [array](https://neon.tech/postgresql/postgresql-tutorial/postgresql-array) in which each value in the set is assigned to an element of the array.
