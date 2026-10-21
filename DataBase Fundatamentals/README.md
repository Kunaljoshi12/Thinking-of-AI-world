# Database Fundamentals — Detailed Guide with Practical SQL Examples

A beginner-friendly database learning guide covering SQL fundamentals, data manipulation, joins, subqueries, database design, transactions, database testing, ETL, and interview problems.

**Databases covered:** MySQL and Microsoft SQL Server (T-SQL). Examples use a common employee-management dataset unless stated otherwise. Some syntax differs between database engines.

## Table of Contents

1. [Database Fundamentals](#1-database-fundamentals)
2. [SQL Command Categories](#2-sql-command-categories)
3. [Database and Table Operations](#3-database-and-table-operations)
4. [Data Retrieval and Filtering](#4-data-retrieval-and-filtering)
5. [SQL Operators and Conditional Logic](#5-sql-operators-and-conditional-logic)
6. [Aggregate Functions](#6-aggregate-functions)
7. [GROUP BY and HAVING](#7-group-by-and-having)
8. [SQL Joins](#8-sql-joins)
9. [Subqueries](#9-subqueries)
10. [Common Table Expressions](#10-common-table-expressions)
11. [Set Operations](#11-set-operations)
12. [Keys and Constraints](#12-keys-and-constraints)
13. [Database Normalization](#13-database-normalization)
14. [Views](#14-views)
15. [Indexes and Query Optimization](#15-indexes-and-query-optimization)
16. [Stored Procedures and Variables](#16-stored-procedures-and-variables)
17. [Triggers and Error Handling](#17-triggers-and-error-handling)
18. [Transactions and ACID Properties](#18-transactions-and-acid-properties)
19. [Date Functions and Conditional Queries](#19-date-functions-and-conditional-queries)
20. [Database Testing](#20-database-testing)
21. [ETL and Data Transformation](#21-etl-and-data-transformation)
22. [SQL Interview Practice](#22-sql-interview-practice)
23. [Additional Concepts to Learn](#23-additional-concepts-to-learn)
24. [Practice Strategy](#24-practice-strategy)

---

## 1. Database Fundamentals

### What is a database?

A database is an organized collection of information that can be stored, retrieved, updated, and managed.

A DBMS (Database Management System) is software that manages databases. An RDBMS stores structured data in related tables.

Examples include MySQL, Microsoft SQL Server, PostgreSQL, and Oracle Database.

**Important terms**

- **Table:** A collection of related records.
- **Row:** One record in a table.
- **Column:** An attribute of a record.
- **Schema:** The logical structure that defines database objects.
- **Relationship:** A connection between tables, often established through keys.
- **Data integrity:** Accuracy and consistency of stored data.

Example: An employee table may contain an employee ID, name, department ID, and salary. A separate department table stores department details.

### Create a database

```sql
CREATE DATABASE CompanyDB;
```

In MySQL:

```sql
USE CompanyDB;
```

In SQL Server, the same `USE CompanyDB;` statement can select an existing database.

**Why use it?** A database separates an application's data from other applications and provides a structured place to store information.

## 2. SQL Command Categories

SQL commands are grouped according to their purpose.

### DDL — Data Definition Language

Used to create or modify database structures.

```sql
CREATE TABLE Departments (
    department_id INT PRIMARY KEY,
    department_name VARCHAR(100)
);
```

Other DDL commands include `ALTER`, `DROP`, and `TRUNCATE`.

### DML — Data Manipulation Language

Used to add, modify, and remove records.

```sql
INSERT INTO Departments
VALUES (1, 'QA');

UPDATE Departments
SET department_name = 'Quality Assurance'
WHERE department_id = 1;

DELETE FROM Departments
WHERE department_id = 1;
```

Always use an appropriate `WHERE` condition when updating or deleting selected records.

### DQL — Data Query Language

Used to retrieve records.

```sql
SELECT * FROM Departments;
```

`SELECT` is commonly categorized as DQL, although some learning resources include it under DML.

### DCL — Data Control Language

Used to manage database permissions.

```sql
GRANT SELECT ON Departments TO analyst_user;
```

The exact syntax and required permissions depend on the database engine.

### TCL — Transaction Control Language

Used to control transactions.

```sql
BEGIN TRANSACTION;

UPDATE Departments
SET department_name = 'QA'
WHERE department_id = 1;

ROLLBACK;
```

The rollback cancels the uncommitted change.

## 3. Database and Table Operations

Database operations include creating tables, inserting records, changing data, and modifying table structures.

### Create a table

```sql
CREATE TABLE Employees (
    employee_id INT PRIMARY KEY,
    employee_name VARCHAR(100) NOT NULL,
    department_id INT,
    salary DECIMAL(10, 2)
);
```

- `INT` stores integer values.
- `VARCHAR(100)` stores variable-length text.
- `DECIMAL(10, 2)` stores a decimal number with two fractional digits.
- `NOT NULL` prevents the employee name from being omitted.
- `PRIMARY KEY` uniquely identifies each employee.

### Insert records

```sql
INSERT INTO Employees
    (employee_id, employee_name, department_id, salary)
VALUES
    (101, 'Amit', 1, 50000.00),
    (102, 'Neha', 2, 65000.00),
    (103, 'Rahul', 1, 45000.00);
```

### Retrieve records

```sql
SELECT * FROM Employees;
```

### Update records

```sql
UPDATE Employees
SET salary = 55000.00
WHERE employee_id = 101;
```

### Delete records

```sql
DELETE FROM Employees
WHERE employee_id = 103;
```

### Modify or remove a table

```sql
ALTER TABLE Employees
ADD email VARCHAR(150);
```

```sql
DROP TABLE Employees;
```

**Important distinction:** `DELETE` removes selected rows, `TRUNCATE` removes all rows, and `DROP` removes the table itself. Their transaction behavior and restrictions vary by database engine.

## 4. Data Retrieval and Filtering

The `SELECT` statement retrieves data from one or more tables.

### Select specific columns

```sql
SELECT employee_name, salary
FROM Employees;
```

Selecting only the required columns can make results easier to read and reduce unnecessary data retrieval.

### Filter with WHERE

```sql
SELECT *
FROM Employees
WHERE salary > 50000;
```

Only employees whose salaries exceed 50,000 are returned.

### Remove duplicates

```sql
SELECT DISTINCT department_id
FROM Employees;
```

`DISTINCT` returns unique department IDs in the result.

### Sort results

```sql
SELECT employee_name, salary
FROM Employees
ORDER BY salary DESC;
```

`DESC` sorts from highest to lowest; `ASC` sorts from lowest to highest.

### Limit results

MySQL:

```sql
SELECT *
FROM Employees
ORDER BY salary DESC
LIMIT 2;
```

SQL Server:

```sql
SELECT TOP 2 *
FROM Employees
ORDER BY salary DESC;
```

Both examples return the two highest-paid employees. Include a unique tie-breaker in `ORDER BY` when deterministic ordering matters.

## 5. SQL Operators and Conditional Logic

Operators allow queries to select records based on conditions.

### Comparison operators

```sql
SELECT *
FROM Employees
WHERE salary >= 50000;
```

Other comparison operators include `=`, `<>`, `!=`, `<`, `>`, and `<=`. `<>` is the standard SQL not-equal operator.

### Logical operators

```sql
SELECT *
FROM Employees
WHERE department_id = 1
  AND salary > 40000;
```

`AND` requires both conditions to be true. `OR` requires at least one condition to be true. `NOT` reverses a condition.

### BETWEEN

```sql
SELECT *
FROM Employees
WHERE salary BETWEEN 40000 AND 60000;
```

`BETWEEN` includes both boundary values.

### IN

```sql
SELECT *
FROM Employees
WHERE department_id IN (1, 2);
```

This returns employees belonging to either department.

### LIKE

```sql
SELECT *
FROM Employees
WHERE employee_name LIKE 'A%';
```

`%` matches zero or more characters, while `_` matches one character in common SQL `LIKE` implementations. Case sensitivity depends on the database and collation.

### NULL handling

```sql
SELECT *
FROM Employees
WHERE department_id IS NULL;
```

Use `IS NULL`, not `= NULL`, to find missing values.

### CASE expression

```sql
SELECT
    employee_name,
    salary,
    CASE
        WHEN salary >= 60000 THEN 'High'
        WHEN salary >= 45000 THEN 'Medium'
        ELSE 'Low'
    END AS salary_category
FROM Employees;
```

`CASE` applies conditional logic to each row and creates a calculated result.

## 6. Aggregate Functions

Aggregate functions calculate a summary from multiple rows.

| Function | Purpose |
|---|---|
| `COUNT()` | Counts rows or non-NULL values, depending on the expression |
| `SUM()` | Adds numeric values |
| `AVG()` | Calculates the average of non-NULL numeric values |
| `MIN()` | Finds the smallest value |
| `MAX()` | Finds the largest value |

Example:

```sql
SELECT
    COUNT(*) AS total_employees,
    SUM(salary) AS total_salary,
    AVG(salary) AS average_salary,
    MIN(salary) AS minimum_salary,
    MAX(salary) AS maximum_salary
FROM Employees;
```

**Why use aggregate functions?** They help produce business reports, salary summaries, sales totals, and record counts without manually processing every row.

Note that `COUNT(*)` counts rows, while `COUNT(salary)` excludes rows where salary is `NULL`.

## 7. GROUP BY and HAVING

`GROUP BY` combines rows with the same grouping value so aggregate functions can calculate results for each group.

```sql
SELECT
    department_id,
    COUNT(*) AS employee_count,
    AVG(salary) AS average_salary
FROM Employees
GROUP BY department_id;
```

This produces one result row per department ID.

### WHERE versus HAVING

- `WHERE` filters rows before grouping.
- `HAVING` filters groups after aggregation.

```sql
SELECT
    department_id,
    COUNT(*) AS employee_count
FROM Employees
WHERE salary >= 40000
GROUP BY department_id
HAVING COUNT(*) >= 2;
```

The query considers employees earning at least 40,000, groups them by department, and returns only departments containing at least two qualifying employees.

## 8. SQL Joins

Joins combine related records from different tables.

Create a department table:

```sql
CREATE TABLE Departments (
    department_id INT PRIMARY KEY,
    department_name VARCHAR(100)
);

INSERT INTO Departments
VALUES
    (1, 'QA'),
    (2, 'Development'),
    (3, 'Support');
```

The following examples use the `Employees` and `Departments` tables defined earlier.

### INNER JOIN

Returns rows that match in both tables.

```sql
SELECT e.employee_name, d.department_name
FROM Employees e
INNER JOIN Departments d
    ON e.department_id = d.department_id;
```

Use it when you need employees with a matching department.

### LEFT JOIN

Returns every row from the left table and matching rows from the right table.

```sql
SELECT e.employee_name, d.department_name
FROM Employees e
LEFT JOIN Departments d
    ON e.department_id = d.department_id;
```

Employees without a matching department remain in the result, with NULL department details.

### RIGHT JOIN

Returns every row from the right table and matching rows from the left table.

```sql
SELECT e.employee_name, d.department_name
FROM Employees e
RIGHT JOIN Departments d
    ON e.department_id = d.department_id;
```

This can show departments that have no employees.

### FULL OUTER JOIN

Returns matched and unmatched rows from both tables.

```sql
SELECT e.employee_name, d.department_name
FROM Employees e
FULL OUTER JOIN Departments d
    ON e.department_id = d.department_id;
```

SQL Server supports `FULL OUTER JOIN`. MySQL does not support it directly; a workaround can combine left and right joins with `UNION`, taking care to avoid unintended duplicates.

### CROSS JOIN

Returns every combination of rows from both tables.

```sql
SELECT e.employee_name, d.department_name
FROM Employees e
CROSS JOIN Departments d;
```

Three employees and three departments produce nine combinations.

### SELF JOIN

A table joins to itself. This is useful for employee-manager relationships.

```sql
SELECT
    e.employee_name AS employee,
    m.employee_name AS manager
FROM Employees e
LEFT JOIN Employees m
    ON e.manager_id = m.employee_id;
```

This example requires a `manager_id` column to exist in `Employees`.

### NATURAL JOIN

A natural join automatically matches columns with the same names. It is database-dependent and can become fragile when schemas change. Explicit `JOIN ... ON` conditions are generally clearer.

## 9. Subqueries

A subquery is a query nested inside another query.

### Subquery with an aggregate

Find employees earning above the overall average salary:

```sql
SELECT employee_name, salary
FROM Employees
WHERE salary > (
    SELECT AVG(salary)
    FROM Employees
);
```

The inner query calculates the average salary; the outer query selects employees above it.

### Subquery with IN

```sql
SELECT employee_name
FROM Employees
WHERE department_id IN (
    SELECT department_id
    FROM Departments
    WHERE department_name IN ('QA', 'Development')
);
```

### Correlated subquery

A correlated subquery references the current row of the outer query.

```sql
SELECT e.employee_name, e.salary, e.department_id
FROM Employees e
WHERE e.salary > (
    SELECT AVG(e2.salary)
    FROM Employees e2
    WHERE e2.department_id = e.department_id
);
```

This returns employees whose salaries exceed their own department's average.

### EXISTS

```sql
SELECT d.department_name
FROM Departments d
WHERE EXISTS (
    SELECT 1
    FROM Employees e
    WHERE e.department_id = d.department_id
);
```

`EXISTS` checks whether at least one matching row exists.

Subqueries are useful for comparisons, membership checks, and complex filtering. Their performance depends on the query, database optimizer, and available indexes.

## 10. Common Table Expressions

A Common Table Expression (CTE) gives a name to a query result that can be referenced by the main statement.

```sql
WITH DepartmentSalary AS (
    SELECT department_id, AVG(salary) AS average_salary
    FROM Employees
    GROUP BY department_id
)
SELECT *
FROM DepartmentSalary
WHERE average_salary > 50000;
```

The CTE first calculates average salary per department. The outer query filters those results.

**Why use CTEs?**

- Improve readability.
- Break a complicated query into logical steps.
- Reuse an intermediate result within the same statement.
- Support recursive queries in databases that implement recursive CTEs.

A CTE is not automatically a materialized or permanently stored table.

## 11. Set Operations

Set operations combine results from separate queries.

### UNION

Removes duplicate result rows.

```sql
SELECT city FROM Customers
UNION
SELECT city FROM Suppliers;
```

### UNION ALL

Retains duplicate rows.

```sql
SELECT city FROM Customers
UNION ALL
SELECT city FROM Suppliers;
```

Both queries require compatible result columns with matching counts and compatible data types.

`UNION ALL` is often preferable when duplicate elimination is unnecessary because it avoids that additional operation.

Other operations, such as `INTERSECT` and `EXCEPT`, return matching or differing sets of rows where supported by the database engine.

## 12. Keys and Constraints

Keys identify records and establish relationships. Constraints enforce rules on stored data.

### Primary key

Uniquely identifies each row and cannot contain NULL values.

```sql
CREATE TABLE Customers (
    customer_id INT PRIMARY KEY,
    customer_name VARCHAR(100) NOT NULL
);
```

### Foreign key

Links a column to a key in another table.

```sql
CREATE TABLE Orders (
    order_id INT PRIMARY KEY,
    customer_id INT,
    FOREIGN KEY (customer_id)
        REFERENCES Customers(customer_id)
);
```

The foreign key prevents an order from referencing a nonexistent customer, subject to the configured constraints.

### UNIQUE

Prevents duplicate values in a constrained column or combination of columns.

```sql
ALTER TABLE Customers
ADD CONSTRAINT UQ_Customers_Name UNIQUE (customer_name);
```

This example assumes customer names must be unique, which may not be a sensible business rule for a real customer database. Email addresses or external identifiers are often more appropriate candidates.

### CHECK

Restricts values to a condition.

```sql
CREATE TABLE Products (
    product_id INT PRIMARY KEY,
    price DECIMAL(10, 2) CHECK (price >= 0)
);
```

### DEFAULT

Supplies a value when an insert omits the column.

```sql
CREATE TABLE Tasks (
    task_id INT PRIMARY KEY,
    status VARCHAR(20) DEFAULT 'Pending'
);
```

Other important concepts include `NOT NULL`, composite keys, candidate keys, and referential integrity.

## 13. Database Normalization

Normalization organizes relational data to reduce unnecessary duplication and prevent data anomalies.

### First Normal Form (1NF)

Each column should contain a single value rather than a repeating list.

Bad design:

| student_id | phone_numbers |
|---|---|
| 1 | 9876, 8765 |

Better design: store phone numbers as separate rows in a related table.

### Second Normal Form (2NF)

A table must satisfy 1NF, and every non-key attribute must depend on the whole candidate key, not just part of a composite key.

### Third Normal Form (3NF)

A table must satisfy 2NF and avoid inappropriate transitive dependencies of non-key attributes on a key.

**Why normalization matters:** It reduces duplicate data and makes insertions, updates, and deletions more reliable.

Normalization must be based on actual dependencies and candidate keys; merely splitting a table into smaller tables does not guarantee a correct design.

## 14. Views

A view is a named query that can be used like a virtual table.

```sql
CREATE VIEW EmployeeDetails AS
SELECT employee_id, employee_name, department_id
FROM Employees;
```

Retrieve data from the view:

```sql
SELECT *
FROM EmployeeDetails;
```

**Why use views?**

- Simplify frequently used queries.
- Present selected columns to users.
- Encapsulate joins and filtering.
- Help control access to underlying data when permissions are configured appropriately.

A normal view does not necessarily store its result data. Materialized views and SQL Server indexed views have different implementation and maintenance characteristics.

## 15. Indexes and Query Optimization

An index is a data structure that can help a database find records without scanning every row.

```sql
CREATE INDEX IX_Employees_Department
ON Employees(department_id);
```

This index may help queries that filter or join on `department_id`.

### Composite index

```sql
CREATE INDEX IX_Employees_Department_Salary
ON Employees(department_id, salary);
```

The column order matters. This index is most directly suited to queries filtering by `department_id`, potentially followed by `salary`.

### Query performance

Check how the database executes a query using the relevant execution-plan tools, such as `EXPLAIN` in MySQL or execution plans in SQL Server.

Indexes are not automatically beneficial. They consume storage and can slow inserts, updates, and deletes because the index structures must also be maintained.

Use execution plans and measurements rather than adding indexes without evidence.

## 16. Stored Procedures and Variables

A stored procedure is a database object containing executable SQL logic.

MySQL example:

```sql
DELIMITER //

CREATE PROCEDURE GetEmployeesByDepartment(
    IN p_department_id INT
)
BEGIN
    SELECT employee_id, employee_name, salary
    FROM Employees
    WHERE department_id = p_department_id;
END //

DELIMITER ;

CALL GetEmployeesByDepartment(1);
```

The procedure accepts a department ID and returns matching employees.

SQL Server uses different syntax, including `CREATE PROCEDURE`, `@`-prefixed parameters, and `EXEC`.

Stored procedures can centralize business logic, accept parameters, and return results. They should still validate inputs and handle errors appropriately.

Variables hold temporary values during SQL execution. Their declaration and scope depend on the database engine and execution context.

## 17. Triggers and Error Handling

A trigger executes automatically when a configured database event occurs.

For example, an audit trigger might record a salary change whenever an employee's salary is updated.

SQL Server example:

```sql
CREATE TABLE EmployeeSalaryAudit (
    employee_id INT,
    old_salary DECIMAL(10, 2),
    new_salary DECIMAL(10, 2),
    changed_at DATETIME2 DEFAULT SYSDATETIME()
);
GO

CREATE TRIGGER trg_EmployeeSalaryAudit
ON Employees
AFTER UPDATE
AS
BEGIN
    SET NOCOUNT ON;

    INSERT INTO EmployeeSalaryAudit (
        employee_id,
        old_salary,
        new_salary
    )
    SELECT
        i.employee_id,
        d.salary,
        i.salary
    FROM inserted i
    INNER JOIN deleted d
        ON i.employee_id = d.employee_id
    WHERE i.salary <> d.salary
       OR (i.salary IS NULL AND d.salary IS NOT NULL)
       OR (i.salary IS NOT NULL AND d.salary IS NULL);
END;
GO
```

SQL Server exposes the affected rows through the logical `inserted` and `deleted` tables. The example handles multiple updated rows.

Error handling captures and responds to failures. SQL Server supports `TRY...CATCH` and `ERROR_MESSAGE()`. MySQL uses its own condition handlers and diagnostics mechanisms.

Triggers should be used carefully because their side effects can make debugging and testing harder.

## 18. Transactions and ACID Properties

A transaction groups database operations into a unit of work.

### ACID

- **Atomicity:** All required operations succeed together or are rolled back.
- **Consistency:** The transaction preserves database rules.
- **Isolation:** Concurrent transactions are controlled according to the configured isolation level.
- **Durability:** Committed changes survive failures according to the database's durability guarantees.

### Example: Transfer between accounts

Assume an `Accounts` table contains `account_id` and `balance`.

```sql
START TRANSACTION;

UPDATE Accounts
SET balance = balance - 1000
WHERE account_id = 1
  AND balance >= 1000;

-- Check that exactly one source row was updated.
-- If not, ROLLBACK and stop.

UPDATE Accounts
SET balance = balance + 1000
WHERE account_id = 2;

-- Verify that the destination row was updated.
-- If not, ROLLBACK and stop.

COMMIT;
```

This is a conceptual MySQL-style example, not a complete production-safe transfer implementation. The application or stored procedure must check affected-row counts, handle errors, and ensure concurrent transfers cannot violate account rules. SQL Server uses different transaction syntax.

Use `ROLLBACK` when the transaction cannot be completed. A `SAVEPOINT` can establish an intermediate rollback point where supported.

## 19. Date Functions and Conditional Queries

Date functions support reporting, filtering, and time-based business rules.

### Filter by a date range

For SQL Server:

```sql
SELECT employee_name, joining_date
FROM Employees
WHERE joining_date >= '2026-01-01'
  AND joining_date < '2027-01-01';
```

This half-open interval includes the entire calendar year even if `joining_date` contains a time component.

### Categorize records

```sql
SELECT
    employee_name,
    salary,
    CASE
        WHEN salary >= 60000 THEN 'High'
        WHEN salary >= 40000 THEN 'Medium'
        ELSE 'Low'
    END AS salary_band
FROM Employees;
```

Date arithmetic and date extraction functions differ between MySQL and SQL Server. Use the correct function for the database engine.

## 20. Database Testing

Database testing checks whether data storage, database logic, constraints, and relationships behave as expected.

### Functional testing

Verify that insert, update, delete, and retrieval operations produce the expected results.

### Data integrity testing

Check primary keys, foreign keys, unique values, required columns, and business rules.

### Data validation testing

Compare expected values with actual database values. Verify record counts, calculated amounts, formats, and relationships.

### Example: Find employees without a valid department

```sql
SELECT e.employee_id, e.employee_name
FROM Employees e
LEFT JOIN Departments d
    ON e.department_id = d.department_id
WHERE e.department_id IS NOT NULL
  AND d.department_id IS NULL;
```

This finds employees with a non-NULL department ID that has no matching department record. With an enforced foreign key, such records should ordinarily be prevented.

### Database testing checklist

- Verify inserted records.
- Verify updated values.
- Verify deleted records.
- Check duplicate and missing data.
- Validate foreign key relationships.
- Verify stored procedure outputs.
- Test trigger side effects.
- Validate rollback behavior.
- Compare source and target data after migration.
- Check database performance under representative loads.
- Test access permissions and SQL injection protections.

For QA automation, Python can execute SQL queries and compare database results with expected values. Database checks can also be integrated into API tests, Playwright workflows, and CI/CD pipelines.

## 21. ETL and Data Transformation

ETL means Extract, Transform, and Load.

- **Extract:** Read data from a source.
- **Transform:** Clean, validate, convert, or calculate values.
- **Load:** Write the transformed data to a target system.

### Example validation query

Suppose `SourceCustomers` and `TargetCustomers` have matching customer IDs and names.

```sql
SELECT
    s.customer_id,
    s.customer_name AS source_name,
    t.customer_name AS target_name
FROM SourceCustomers s
INNER JOIN TargetCustomers t
    ON s.customer_id = t.customer_id
WHERE s.customer_name <> t.customer_name
   OR (s.customer_name IS NULL AND t.customer_name IS NOT NULL)
   OR (s.customer_name IS NOT NULL AND t.customer_name IS NULL);
```

This query identifies matched customer records whose names differ, including NULL mismatches.

Additional ETL validation includes comparing source and target row counts, checking duplicates, validating transformed amounts, and identifying missing target records.

Pentaho Data Integration is a tool for building data extraction and transformation workflows. A transformation file alone does not establish that every ETL concept has been practiced.

## 22. SQL Interview Practice

Solve each problem using the appropriate SQL techniques.

### Find the second-highest distinct salary

```sql
SELECT MAX(salary) AS second_highest_salary
FROM Employees
WHERE salary < (
    SELECT MAX(salary)
    FROM Employees
);
```

This returns the second-highest distinct salary, or NULL if no second distinct value exists.

### Find employees above their department's average

```sql
SELECT e.employee_name, e.salary, e.department_id
FROM Employees e
WHERE e.salary > (
    SELECT AVG(e2.salary)
    FROM Employees e2
    WHERE e2.department_id = e.department_id
);
```

### Find departments with more than two employees

```sql
SELECT department_id, COUNT(*) AS employee_count
FROM Employees
GROUP BY department_id
HAVING COUNT(*) > 2;
```

### Additional practice problems

1. Find the highest-paid employee in each department.
2. Find duplicate records.
3. Find customers who have never placed an order.
4. Find employees without a matching department.
5. Calculate total sales per customer.
6. Find departments with above-average salaries.
7. Retrieve the latest record for each customer.
8. Compare data between source and target tables.
9. Find missing records between two datasets.
10. Calculate cumulative totals using window functions.

## 23. Additional Concepts to Learn

These topics extend the existing practice material and should be marked as completed only after hands-on exercises.

### Advanced SQL

- Window functions: `ROW_NUMBER()`, `RANK()`, and `DENSE_RANK()`
- `LAG()` and `LEAD()`
- Running totals
- Recursive CTEs
- Advanced string and date functions
- Execution-plan analysis

### Database design

- Entity-Relationship diagrams
- One-to-one, one-to-many, and many-to-many relationships
- Functional dependencies
- OLTP versus OLAP
- Data warehouse fundamentals

### Transactions and reliability

- Isolation levels
- Dirty reads, non-repeatable reads, and phantom reads
- Locks and deadlocks
- Backup and recovery
- Database migration strategies

### Python and QA automation

- Connecting Python to MySQL or SQL Server
- Executing parameterized queries
- Validating database results with automated tests
- Generating test data
- Integrating database checks into Playwright tests
- Running database validations in CI/CD pipelines

## 24. Practice Strategy

Follow this order to build practical skills:

1. Create databases and tables.
2. Practice `SELECT`, filtering, sorting, inserts, updates, and deletes.
3. Solve aggregate and grouping problems.
4. Practice joins using related tables.
5. Solve subquery and CTE problems.
6. Practice keys and constraints.
7. Work with views, indexes, procedures, and triggers.
8. Test transactions and rollback scenarios.
9. Write database testing queries.
10. Practice ETL source-to-target validation.
11. Solve interview questions without looking at solutions.
12. Automate database validation using Python.

### Repository Guidelines

- Keep separate SQL scripts for different topics.
- Include sample data and expected results.
- Use meaningful table and column names.
- Test positive and negative scenarios.
- Document assumptions and business rules.
- Separate MySQL and SQL Server syntax when necessary.
- Mark topics as completed only after implementing and testing them.

**Learning objective:** Develop practical SQL skills for database development, QA engineering, data validation, and automated testing.
