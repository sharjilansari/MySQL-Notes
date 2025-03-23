# SQL Notes

## Table of Contents

1. [Data Types](#data-types)
   - [Numeric Data Types](#numeric-data-types)
   - [Numeric Type Attributes](#numeric-type-attributes)
   - [Date and Time Data Types](#date-and-time-data-types)
     - [TIMESTAMP and DATETIME](#timestamp-and-datetime)
     - [DATE](#date)
     - [TIME](#time)
     - [YEAR](#year)
   - [String Data Types](#string-data-types)
     - [Character String Data Types](#character-string-data-types)
       - [CHAR(n)](#charn)
       - [VARCHAR(n)](#varcharn)
       - [TEXT](#text)
     - [Binary String Data Types](#binary-string-data-types)
       - [BLOB](#blob)
     - [Common String Functions](#common-string-functions)
2. [MySQL Operators and Functions](#mysql-operators-and-functions)
   - [Assignment Operators](#assignment-operators)
   - [Arithmetic Operators](#arithmetic-operators)
   - [Comparison Operators](#comparison-operators)
   - [Logical Operators](#logical-operators)
   - [Bitwise Operators](#bitwise-operators)
   - [String Functions](#string-functions)
   - [Numeric Functions](#numeric-functions)
   - [Date and Time Functions](#date-and-time-functions)
   - [Aggregate Functions](#aggregate-functions)
   - [Control Flow Functions](#control-flow-functions)
3. [Data Definition Statements](#data-definition-statements)
   - [CREATE TABLE](#create-table)
   - [ALTER TABLE](#alter-table)
   - [DROP TABLE](#drop-table)
4. [Data Manipulation Statements](#data-manipulation-statements)
   - [INSERT](#insert)
   - [SELECT](#select)
   - [UPDATE](#update)
   - [DELETE](#delete)
5. [Transactional Statements](#transactional-statements)
   - [START TRANSACTION](#start-transaction)
   - [COMMIT](#commit)
   - [ROLLBACK](#rollback)
6. [Prepared Statements](#prepared-statements)
   - [PREPARE](#prepare)
   - [EXECUTE](#execute)
   - [DEALLOCATE PREPARE](#deallocate-prepare)
7. [Compound Statements](#compound-statements)
   - [BEGIN ... END](#begin-end)
   - [IF ... THEN](#if-then)
   - [LOOP](#loop)
8. [Server Administration Statements](#server-administration-statements)
   - [SHOW DATABASES](#show-databases)
   - [SHOW TABLES](#show-tables)
   - [SHOW PROCESSLIST](#show-processlist)
9. [Utility Statements](#utility-statements)
   - [DESCRIBE](#describe)
   - [EXPLAIN](#explain)
   - [USE](#use)
10. [Clauses](#clauses)
   - [WHERE](#where)
   - [GROUP BY](#group-by)
   - [HAVING](#having)
   - [ORDER BY](#order-by)
   - [LIMIT](#limit)
   - [JOIN](#join)


## Data Types

### Numeric data types

1. `BIT(M)` :
A bit-value type. M indicates the number of bits per value, from 1 to 64. The default is 1 if M is omitted. Storage bytes 1

2. `TINYINT(M)` :
A very small integer. The signed range is -128 to 127. The unsigned range is 0 to 255. Storage bytes 1

3. `BOOL, BOOLEAN` : 
These types are synonyms for TINYINT(1). A value of zero is considered false. Nonzero values are considered true. Storage bytes 1

4. `INT(M) or INTEGER(M)` :
A normal-size integer. The signed range is -2^31 to 2^31-1. The unsigned range is 0 to 2^32-1. Storage bytes 4

5. `BIGINT(M)` :
A large integer. The signed range is -2^63 to 2^63-1. The unsigned range is 0 to 2^64-1. Storage bytes 8

- NOTE: SERIAL is an alias for BIGINT UNSIGNED NOT NULL AUTO_INCREMENT UNIQUE.

6. `FLOAT(M,D)` :
Where M is total no of digits and D is no. of digits that can exist after decimal point.(A single-precision floating-point number is accurate to approximately 7 decimal places.). RANGE: -3.4 x 10^38 to 3.4 x 10^38.

7. `DOUBLE(M,D)` :
M and D are same as float. -1.7 x 10^308 to 1.7 x 10^308.

8. `DECIMAL(M,D)`

### Numeric Type Attributes

1. UNSIGNED : An unsigned type can be used to permit only nonnegative numbers.
2. AUTO_INCREMENT : Increases the value by +1.
#### More about AUTO_INCREMENT:
- AUTO_INCREMENT columns must be NOT NULL to generate values.
- Inserting NULL into an AUTO_INCREMENT NOT NULL column generates the next number.
- Manually inserting a value resets the sequence from that value.
- If the column allows NULL, MySQL does NOT auto-generate values.
- AUTO_INCREMENT for FLOAT and DOUBLE columns is not supported.

---

### Date and Time DAta Types

there are four for it - `DATE`, `TIME`, `DATETIME`, `TIMESTAMP` and `YEAR`.

`TIMESTAMP` and `DATETIME` columns can be automatically initialized and updated to the current date and time (that is, the current timestamp).

Using `TIMESTAMP` & `DATETIME`:

```sql
CREATE TABLE orders (
    order_id INT AUTO_INCREMENT PRIMARY KEY,
    order_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);
```
- `DEFAULT CURRENT_TIMESTAMP`: Sets the initial value to the current date & time when inserting a row.

- `ON UPDATE CURRENT_TIMESTAMP`: Updates the value automatically whenever the row is updated.

- NOTE: `DATETIME` does not support automatic initialization and updates.
- NOTE: In MySQL, when you define a `TIMESTAMP` column with `ON UPDATE CURRENT_TIMESTAMP` but without `DEFAULT CURRENT_TIMESTAMP`, the column only updates automatically when the row is modified but does not get a default value when inserting a new row.

```sql
CREATE TABLE t1 (
  ts1 TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,     -- default 0
  ts2 TIMESTAMP NULL ON UPDATE CURRENT_TIMESTAMP -- default NULL
  CREATE TABLE t1 (ts TIMESTAMP NULL DEFAULT '0000-00-00 00:00:00');
);
```

To set the TIMESTAMP column in either table to the current timestamp at insert time, explicitly assign it that value. For example:
```sql
INSERT INTO t2 VALUES (CURRENT_TIMESTAMP);
INSERT INTO t1 VALUES (NOW());
```
Using `DATE` :
- DATE is a data type used to store only date values (without time).
- Format: YYYY-MM-DD (e.g., 2024-03-23).
- Range: '1000-01-01' to '9999-12-31'.
- Default storage: 3 bytes.
```sql
CREATE TABLE employees (
    birth_date DATE --Dates must be in YYYY-MM-DD format (ISO 8601 standard)
);
SELECT CURDATE();  -- Returns today's date
SELECT DATE_FORMAT('2024-03-23', '%d-%b-%Y'); --23-Mar-2024
order_date DATE NOT NULL DEFAULT (CURDATE()) --order_date automatically stores today’s date if no value is given.
```
- `%d` → Day
- `%b` → Short month name
- `%Y` → Full year

### Date Type Operations
1. DATE_ADD() - adds pecific no. of days
2. DATEDIFF() - Calculates days between two dates.
3. CURDATE() - gives current date.

Using `TIME`:
- Stores only time (hours, minutes, seconds).
- Format: HH:MM:SS (e.g., 14:30:15).
- Storage: 3 bytes.
```sql
CREATE TABLE work_shifts (
    shift_start TIME,
    shift_end TIME
);
```
```sql
INSERT INTO work_shifts (shift_start, shift_end) VALUES 
('09:00:00', '17:00:00'),
( '22:00:00', '06:00:00');
```
### Time Type Operations
1. TIMEDIFF() - gives differnce between two times
2. SEC_TO_TIME() - converts second to hours
3. CURTIME() - gives current time

Using `YEAR`:
- Stores only the year.
- Format: YYYY (e.g., 2024).
- Range: 1901 to 2155.
- Storage: 1 byte.
```sql
CREATE TABLE employees (
    joining_year YEAR
);
```
```sql
INSERT INTO employees (joining_year) VALUES 
(2020),
(2015);
```
### Year Type Operations
1. YEAR(CURDATE()) - gives current year
2. YEAR('2023-08-15') - Extracting Year from Date

### **String Data Types**

MySQL provides several string (character) data types to store **text, numbers, or binary data**. These are divided into **character strings** and **binary strings**.

---

### **Character String Data Types**
These store **textual data** (letters, numbers, symbols). They use different storage strategies.

### **1. `CHAR(n)` - Fixed-Length String**
- Stores **exactly `n` characters**, **right-padded** with spaces if shorter.
- **Fast retrieval** but wastes space if values are shorter than `n`.
- **Storage:** `n` bytes.
- **Max Length:** `255` characters.

#### **Example**
```sql
CREATE TABLE users (
    id INT PRIMARY KEY AUTO_INCREMENT,
    username CHAR(10)
);
INSERT INTO users (username) VALUES ('John');  -- Stored as 'John      '
```

**Best for**: Fixed-length data (e.g., **country codes, hashes, gender**).

---

### **2. `VARCHAR(n)` - Variable-Length String**
- Stores up to `n` characters, only using **needed space** (+1 or 2 bytes overhead).
- **Faster than `TEXT` but slower than `CHAR`**.
- **Max Length:** `65,535` (but depends on row size).

#### **Example**
```sql
CREATE TABLE employees (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(50)
);
INSERT INTO employees (name) VALUES ('Alice');  -- Stored as 'Alice'
```

**Best for**: Names, emails, descriptions where length varies.

---

### **3. `TEXT` - Large Text Storage**
- Stores large amounts of text (`VARCHAR` has limits based on row size).
- **Cannot have a default value**.
- **Stored separately from row data**.
- **Max Lengths:**
  - `TINYTEXT` → 255 chars (~255 bytes).
  - `TEXT` → 65,535 chars (~64 KB).
  - `MEDIUMTEXT` → 16,777,215 chars (~16 MB).
  - `LONGTEXT` → 4,294,967,295 chars (~4 GB).

#### **Example**
```sql
CREATE TABLE articles (
    id INT PRIMARY KEY AUTO_INCREMENT,
    content TEXT
);
```

**Best for**: Articles, descriptions, logs, comments.

---

### **Binary String Data Types**
These store **raw binary data** (images, files, encrypted text).

### **4. `BLOB` (Binary Large Object)**
- Like `TEXT`, but stores **binary data**.
- Used for **images, PDFs, encrypted files**.
- **Max Lengths**:  
  - `TINYBLOB` → 255 bytes  
  - `BLOB` → 64 KB  
  - `MEDIUMBLOB` → 16 MB  
  - `LONGBLOB` → 4 GB  

#### **Example**
```sql
CREATE TABLE files (
    id INT PRIMARY KEY AUTO_INCREMENT,
    file_data BLOB
);
```

**Best for**: Storing images, audio, video, encrypted passwords.

---

### **Choosing the Right String Type**
| Data Type   | Max Length | Use Case |
|------------|------------|-----------------|
| `CHAR(n)` | 255 chars | Fixed-length data like country codes |
| `VARCHAR(n)` | 65,535 chars | Variable-length text like names, emails |
| `TEXT` | 64 KB – 4 GB | Long text like articles, logs |
| `BLOB` | 64 KB – 4 GB | Binary data like images, files |

---

### **Common String Functions**
#### **Searching in Strings**
```sql
SELECT * FROM employees WHERE name LIKE 'A%';  -- Names starting with A
```

#### **String Length**
```sql
SELECT LENGTH('Hello');  -- Output: 5 (bytes)
```

#### **Convert to Uppercase/Lowercase**
```sql
SELECT UPPER('hello');  -- Output: HELLO
SELECT LOWER('HELLO');  -- Output: hello
```

#### **Substring Extraction**
```sql
SELECT SUBSTRING('Database', 1, 4);  -- Output: "Data"
```

#### **Replace Text**
```sql
SELECT REPLACE('Hello World', 'World', 'MySQL');  -- Output: Hello MySQL
```

---

## MySQL Operators and Functions

## Assignment Operators

| Operator | Description | Example |
|----------|-------------|---------|
| `=`      | Assign a value (used in `SET` and `UPDATE` statements) | `SET a = b` |
| `:=`     | Assign a value | `a := b` |

## Arithmetic Operators

| Operator | Description | Example |
|----------|-------------|---------|
| `+`      | Addition | `a + b` |
| `-`      | Subtraction | `a - b` |
| `*`      | Multiplication | `a * b` |
| `/`      | Division | `a / b` |
| `%`      | Modulus | `a % b` |
| `DIV`    | Integer division | `a DIV b` |
| `-`      | Unary minus (negation) | `-a` |

## Comparison Operators

| Operator | Description | Example |
|----------|-------------|---------|
| `=`      | Equal to | `a = b` |
| `>`      | Greater than | `a > b` |
| `<`      | Less than | `a < b` |
| `>=`     | Greater than or equal to | `a >= b` |
| `<=`     | Less than or equal to | `a <= b` |
| `<>`, `!=` | Not equal to | `a <> b` |
| `<=>`    | NULL-safe equal to | `a <=> b` |
| `IS NULL` | Checks if a value is NULL | `a IS NULL` |
| `IS NOT NULL` | Checks if a value is NOT NULL | `a IS NOT NULL` |
| `BETWEEN ... AND ...` | Checks if value is in range | `a BETWEEN 1 AND 10` |
| `IN()`   | Checks if value is in list | `a IN (1, 2, 3)` |
| `LIKE`   | Simple pattern matching | `a LIKE 'pattern%'` |
| `NOT LIKE` | Negation of LIKE | `a NOT LIKE 'pattern%'` |
| `REGEXP` | Regex pattern match | `a REGEXP '^[A-Z]'` |
| `NOT REGEXP` | Negation of REGEXP | `a NOT REGEXP '^[A-Z]'` |
| `SOUNDS LIKE` | Compare sound similarity | `a SOUNDS LIKE b` |
| `STRCMP()` | Compares two strings (returns -1, 0, 1) | `STRCMP('abc', 'abd')` |

## Logical Operators

| Operator | Description | Example |
|----------|-------------|---------|
| `AND`, `&&` | Logical AND | `a AND b` |
| `OR`, `||`  | Logical OR | `a OR b` |
| `NOT`, `!`  | Logical NOT | `NOT a` |
| `XOR`       | Logical XOR | `a XOR b` |

## Bitwise Operators

| Operator | Description | Example |
|----------|-------------|---------|
| `&`      | Bitwise AND | `a & b` |
| `|`      | Bitwise OR | `a | b` |
| `^`      | Bitwise XOR | `a ^ b` |
| `~`      | Bitwise NOT | `~a` |
| `<<`     | Left shift | `a << b` |
| `>>`     | Right shift | `a >> b` |

## String Functions

| Function | Description | Example | Result |
|----------|------------|---------|--------|
| `CONCAT()` | Concatenates strings | `CONCAT('Hello', ' ', 'World')` | `Hello World` |
| `LENGTH()` | String length (bytes) | `LENGTH('Hello')` | `5` |
| `LOWER()` | Converts to lowercase | `LOWER('HELLO')` | `hello` |
| `UPPER()` | Converts to uppercase | `UPPER('hello')` | `HELLO` |
| `SUBSTRING()` | Extracts substring | `SUBSTRING('Hello World', 7, 5)` | `World` |
| `TRIM()` | Removes spaces | `TRIM(' Hello ')` | `Hello` |
| `REPLACE()` | Replaces substring | `REPLACE('Hello World', 'World', 'MySQL')` | `Hello MySQL` |
| `INSTR()` | Finds substring position | `INSTR('Hello World', 'World')` | `7` |
| `LEFT()` | Leftmost `n` characters | `LEFT('Hello World', 5)` | `Hello` |
| `RIGHT()` | Rightmost `n` characters | `RIGHT('Hello World', 5)` | `World` |

## Numeric Functions

| Function | Description | Example |
|----------|------------|---------|
| `ABS()`  | Absolute value | `ABS(-5)` |
| `CEIL()` | Round up | `CEIL(4.2)` |
| `FLOOR()` | Round down | `FLOOR(4.8)` |
| `ROUND()` | Rounds number | `ROUND(4.5)` |
| `MOD()`  | Modulus | `MOD(10, 3)` |

## Date and Time Functions

| Function | Description | Example |
|----------|------------|---------|
| `NOW()`  | Current date & time | `NOW()` |
| `CURDATE()` | Current date | `CURDATE()` |
| `CURTIME()` | Current time | `CURTIME()` |
| `DATE_ADD()` | Add interval to date | `DATE_ADD('2025-03-23', INTERVAL 1 DAY)` |
| `DATEDIFF()` | Difference between dates | `DATEDIFF('2025-03-23', '2025-03-20')` |

## Aggregate Functions

| Function | Description | Example |
|----------|------------|---------|
| `COUNT()` | Count rows | `COUNT(*)` |
| `SUM()`   | Sum of values | `SUM(column_name)` |
| `AVG()`   | Average of values | `AVG(column_name)` |
| `MIN()`   | Minimum value | `MIN(column_name)` |
| `MAX()`   | Maximum value | `MAX(column_name)` |

## Control Flow Functions

| Function | Description | Example |
|----------|------------|---------|
| `CASE`   | Case condition | `CASE WHEN condition THEN result ELSE result END` |
| `IF()`   | If-else construct | `IF(condition, true_value, false_value)` |
| `IFNULL()` | Returns alternate value if NULL | `IFNULL(expr, alt_value)` |
| `NULLIF()` | Returns NULL if values are equal | `NULLIF(a, b)` |

---

## Data Definition Statements

### CREATE TABLE

Creates a new table in the database.

```sql
CREATE TABLE employees (
    id INT PRIMARY KEY,
    name VARCHAR(50),
    position VARCHAR(50)
);
```

**Output:**

```
Query OK, 0 rows affected
```

### ALTER TABLE

Modifies an existing table structure.

- **Add a column**

```sql
ALTER TABLE employees ADD COLUMN salary DECIMAL(10, 2);
```

- **Modify a column**

```sql
ALTER TABLE employees MODIFY COLUMN salary INT;
```

- **Rename a column**

```sql
ALTER TABLE employees CHANGE COLUMN salary employee_salary DECIMAL(10,2);
```

- **Drop a column**

```sql
ALTER TABLE employees DROP COLUMN employee_salary;
```

- **Rename the table**

```sql
ALTER TABLE employees RENAME TO staff;
```

**Output:**

```
Query OK, 0 rows affected
Records: 0  Duplicates: 0  Warnings: 0
```

### DROP TABLE

Deletes a table from the database.

```sql
DROP TABLE employees;
```

**Output:**

```
Query OK, 0 rows affected
```

## Data Manipulation Statements

### INSERT

Adds new rows to a table.

```sql
INSERT INTO employees (id, name, position, salary) VALUES (1, 'Alice', 'Manager', 75000.00);
```

**Output:**

```
Query OK, 1 row affected
```

### SELECT

Retrieves data from one or more tables.

```sql
SELECT * FROM employees;
```

**Output:**

```
+----+-------+----------+--------+
| id | name  | position | salary |
+----+-------+----------+--------+
|  1 | Alice | Manager  | 75000.00 |
+----+-------+----------+--------+
```

### UPDATE

Modifies existing data within a table.

```sql
UPDATE employees SET salary = 80000.00 WHERE id = 1;
```

**Output:**

```
Query OK, 1 row affected
```

### DELETE

Removes rows from a table.

```sql
DELETE FROM employees WHERE id = 1;
```

**Output:**

```
Query OK, 1 row affected
```

## Transactional Statements

### START TRANSACTION

Begins a new transaction.

```sql
START TRANSACTION;
```

**Output:**

```
Query OK, 0 rows affected
```

### COMMIT

Commits the current transaction, making all changes permanent.

```sql
COMMIT;
```

**Output:**

```
Query OK, 0 rows affected
```

### ROLLBACK

Rolls back the current transaction, undoing all changes.

```sql
ROLLBACK;
```

**Output:**

```
Query OK, 0 rows affected
```

## Prepared Statements

### PREPARE

Prepares an SQL statement for execution.

```sql
PREPARE stmt FROM 'SELECT * FROM employees WHERE position = ?';
```

**Output:**

```
Query OK, 0 rows affected
Statement prepared
```

### EXECUTE

Executes a prepared statement.

```sql
SET @position = 'Manager';
EXECUTE stmt USING @position;
```

**Output:**

```
+----+-------+----------+--------+
| id | name  | position | salary |
+----+-------+----------+--------+
|  1 | Alice | Manager  | 80000.00 |
+----+-------+----------+--------+
```

### DEALLOCATE PREPARE

Deallocates a prepared statement.

```sql
DEALLOCATE PREPARE stmt;
```

**Output:**

```
Query OK, 0 rows affected
```

## Compound Statements

### BEGIN ... END

Defines a block of statements.

```sql
BEGIN
    DECLARE total_employees INT;
    SELECT COUNT(*) INTO total_employees FROM employees;
END;
```

**Output:**

```
Query OK, 0 rows affected
```

### IF ... THEN

Conditional control flow within a compound statement.

```sql
IF total_employees > 0 THEN
    SELECT 'Employees exist';
ELSE
    SELECT 'No employees';
END IF;
```

**Output:**

```
+------------------+
| Employees exist  |
+------------------+
```

### LOOP

Repeats a block of statements.

```sql
LOOP
    -- statements
    LEAVE loop_label;
END LOOP;
```

**Output:**

```
Query OK, 0 rows affected
```

## Server Administration Statements

### SHOW DATABASES

Lists all databases in the MySQL server.

```sql
SHOW DATABASES;
```

**Output:**
```
+--------------------+
| Database          |
+--------------------+
| information_schema |
| company_db        |
| test_db           |
+--------------------+
```

### SHOW TABLES

Lists all tables in the currently selected database.

```sql
SHOW TABLES;
```

**Output:**
```
+------------------+
| Tables_in_dbname |
+------------------+
| employees        |
| departments      |
+------------------+
```

### SHOW PROCESSLIST

Displays the currently active processes and queries running in MySQL.

```sql
SHOW PROCESSLIST;
```

**Output:**
```
+----+------+-----------+------+---------+------+-------+------------------+
| Id | User | Host      | db   | Command | Time | State | Info             |
+----+------+-----------+------+---------+------+-------+------------------+
|  1 | root | localhost | NULL | Sleep   |  10  |       | NULL             |
|  2 | root | localhost | test | Query   |   0  | NULL  | SHOW PROCESSLIST |
+----+------+-----------+------+---------+------+-------+------------------+
```

## Utility Statements

### DESCRIBE

Displays the structure of a table, including column names, data types, and constraints.

```sql
DESCRIBE employees;
```

**Output:**
```
+------------+-------------+------+-----+---------+-------+
| Field      | Type        | Null | Key | Default | Extra |
+------------+-------------+------+-----+---------+-------+
| id         | INT         | NO   | PRI | NULL    |       |
| name       | VARCHAR(50) | NO   |     | NULL    |       |
| position   | VARCHAR(50) | NO   |     | NULL    |       |
| salary     | DECIMAL(10,2) | NO  |     | NULL    |       |
+------------+-------------+------+-----+---------+-------+
```

### EXPLAIN

Provides information about how MySQL executes a query, useful for performance tuning.

```sql
EXPLAIN SELECT * FROM employees WHERE salary > 50000;
```

**Output:**
```
+----+-------------+----------+------------+------+---------------+------+---------+------+------+----------+-------+
| id | select_type | table    | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra |
+----+-------------+----------+------------+------+---------------+------+---------+------+------+----------+-------+
|  1 | SIMPLE      | employees| NULL       | ALL  | NULL          | NULL | NULL    | NULL |  100 |    50.00 | Using where |
+----+-------------+----------+------------+------+---------------+------+---------+------+------+----------+-------+
```

### USE

Switches the active database to the specified one.

```sql
USE company_db;
```

**Output:**
```
Database changed
```

## Access Control Statements

### GRANT

Grants specific privileges to a user.

```sql
GRANT SELECT, INSERT ON company_db.* TO 'user1'@'localhost';
```

**Output:**
```
Query OK, 0 rows affected
```

### REVOKE

Removes specific privileges from a user.

```sql
REVOKE INSERT ON company_db.* FROM 'user1'@'localhost';
```

**Output:**
```
Query OK, 0 rows affected
```

### CREATE USER

Creates a new user in the MySQL server.

```sql
CREATE USER 'user1'@'localhost' IDENTIFIED BY 'password123';
```

**Output:**
```
Query OK, 0 rows affected
```

# Clauses

## WHERE Clause
The WHERE clause is used to filter records based on a specific condition. It allows you to specify criteria to retrieve only the rows that satisfy the condition.

### Syntax
```sql
SELECT column1, column2, ...
FROM table_name
WHERE condition;
```

### Example
```sql
SELECT * FROM employees WHERE salary > 50000;
```
#### Output:
| id | name  | salary  | department  |
|----|-------|---------|------------|
| 1  | John  | 60000   | Sales      |
| 2  | Alice | 70000   | IT         |

### Advanced Usage
#### Combining Conditions with AND/OR:
```sql
SELECT * FROM employees WHERE salary > 50000 AND department = 'Sales';
```
#### Output:
| id | name  | salary  | department  |
|----|-------|---------|------------|
| 1  | John  | 60000   | Sales      |

#### Using IN:
```sql
SELECT * FROM employees WHERE department IN ('Sales', 'Marketing');
```
#### Output:
| id | name  | department  |
|----|-------|------------|
| 1  | John  | Sales      |
| 3  | Bob   | Marketing  |

#### Using BETWEEN:
```sql
SELECT * FROM employees WHERE salary BETWEEN 40000 AND 60000;
```
#### Output:
| id | name  | salary  |
|----|-------|---------|
| 4  | Mike  | 50000   |

#### Using LIKE for Pattern Matching:
```sql
SELECT * FROM employees WHERE name LIKE 'J%';
```
#### Output:
| id | name  |
|----|-------|
| 1  | John  |
| 5  | James |

---

## GROUP BY Clause
The GROUP BY clause groups rows with the same values in specified columns. It is often used with aggregate functions like COUNT, SUM, AVG, MIN, and MAX.

### Syntax
```sql
SELECT column1, aggregate_function(column2)
FROM table_name
GROUP BY column1;
```

### Example
```sql
SELECT department, COUNT(*) FROM employees GROUP BY department;
```
#### Output:
| department | count |
|------------|-------|
| Sales      | 2     |
| IT         | 1     |
| Marketing  | 1     |

### Advanced Usage
#### Grouping by Multiple Columns:
```sql
SELECT department, gender, COUNT(*) 
FROM employees 
GROUP BY department, gender;
```
#### Output:
| department | gender | count |
|------------|--------|-------|
| Sales      | Male   | 1     |
| IT         | Female | 1     |

#### Using GROUP BY with HAVING:
```sql
SELECT department, AVG(salary) 
FROM employees 
GROUP BY department 
HAVING AVG(salary) > 60000;
```
#### Output:
| department | avg_salary |
|------------|------------|
| IT         | 70000      |

---

## HAVING Clause
The HAVING clause is used to filter grouped results after the GROUP BY clause. It is similar to WHERE but is used for aggregated data.

### Syntax
```sql
SELECT column1, aggregate_function(column2)
FROM table_name
GROUP BY column1
HAVING condition;
```

### Example
```sql
SELECT department, AVG(salary) 
FROM employees 
GROUP BY department 
HAVING AVG(salary) > 60000;
```
#### Output:
| department | avg_salary |
|------------|------------|
| IT         | 70000      |

---

## ORDER BY Clause
The ORDER BY clause is used to sort the result set in ascending (ASC) or descending (DESC) order based on one or more columns.

### Syntax
```sql
SELECT column1, column2, ...
FROM table_name
ORDER BY column1 ASC|DESC, column2 ASC|DESC;
```

### Example
```sql
SELECT * FROM employees ORDER BY salary DESC;
```
#### Output:
| id | name  | salary  |
|----|-------|---------|
| 2  | Alice | 70000   |
| 1  | John  | 60000   |

#### Sorting by Multiple Columns:
```sql
SELECT * FROM employees ORDER BY department ASC, salary DESC;
```
#### Output:
| id | name  | department | salary  |
|----|-------|------------|---------|
| 3  | Bob   | Marketing  | 50000   |
| 1  | John  | Sales      | 60000   |

#### Using ORDER BY with LIMIT:
```sql
SELECT * FROM employees ORDER BY salary DESC LIMIT 5;
```
#### Output:
| id | name  | salary  |
|----|-------|---------|
| 2  | Alice | 70000   |
| 1  | John  | 60000   |

---

## LIMIT Clause
The LIMIT clause is used to restrict the number of records returned by a query.

### Syntax
```sql
SELECT column1, column2, ...
FROM table_name
LIMIT number;
```

### Example
```sql
SELECT * FROM employees LIMIT 5;
```
#### Output:
| id | name  |
|----|-------|
| 1  | John  |
| 2  | Alice |

#### Using LIMIT with OFFSET:
```sql
SELECT * FROM employees LIMIT 5 OFFSET 10;
```
#### Output:
(Skips the first 10 rows and retrieves the next 5 rows)

---

## JOIN Clause
The JOIN clause is used to combine rows from two or more tables based on a related column between them.

### Example
```sql
SELECT employees.name, departments.department_name 
FROM employees 
JOIN departments ON employees.department_id = departments.id;
```
#### Output:
| name  | department_name |
|-------|----------------|
| John  | Sales          |
| Alice | IT             |

#### Using Aliases:
```sql
SELECT e.name, d.department_name 
FROM employees AS e
JOIN departments AS d ON e.department_id = d.id;
```
#### Output:
| name  | department_name |
|-------|----------------|
| John  | Sales          |
| Alice | IT             |

#### Multiple Joins:
```sql
SELECT e.name, d.department_name, p.project_name 
FROM employees AS e
JOIN departments AS d ON e.department_id = d.id
JOIN projects AS p ON e.project_id = p.id;
```
#### Output:
| name  | department_name | project_name |
|-------|----------------|--------------|
| John  | Sales          | Project A    |
| Alice | IT             | Project B    |

---

## Summary of Clauses
| Clause    | Purpose                                    | Example |
|-----------|--------------------------------------------|---------|
| WHERE     | Filters rows based on a condition         | `SELECT * FROM employees WHERE salary > 50000;` |
| GROUP BY  | Groups rows with the same values         | `SELECT department, COUNT(*) FROM employees GROUP BY department;` |
| HAVING    | Filters grouped results after GROUP BY    | `SELECT department, AVG(salary) FROM employees GROUP BY department HAVING AVG(salary) > 60000;` |
| ORDER BY  | Sorts results in ascending or descending order | `SELECT * FROM employees ORDER BY salary DESC;` |
| LIMIT     | Limits the number of rows returned        | `SELECT * FROM employees LIMIT 5;` |
| JOIN      | Combines rows from two or more tables     | `SELECT employees.name, departments.department_name FROM employees JOIN departments ON employees.department_id = departments.id;` |