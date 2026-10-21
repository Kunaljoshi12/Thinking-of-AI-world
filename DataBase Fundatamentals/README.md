# Database Fundamentals and SQL Practice

A practical learning repository covering database fundamentals, SQL queries, database design, advanced querying, database testing, ETL, and SQL interview problems.

The repository contains SQL practice examples using employee, department, customer, order, student, and other sample datasets. Some topics are represented by hands-on SQL exercises, while others are conceptual learning and interview preparation topics.

## Table of Contents

1. [Database Fundamentals](#1-database-fundamentals)
2. [SQL Command Categories](#2-sql-command-categories)
3. [Database and Table Operations](#3-database-and-table-operations)
4. [SQL Data Retrieval and Filtering](#4-sql-data-retrieval-and-filtering)
5. [SQL Operators and Conditional Logic](#5-sql-operators-and-conditional-logic)
6. [SQL Aggregate Functions](#6-sql-aggregate-functions)
7. [GROUP BY and HAVING](#7-group-by-and-having)
8. [SQL Joins](#8-sql-joins)
9. [Subqueries](#9-subqueries)
10. [Common Table Expressions](#10-common-table-expressions)
11. [Set Operations](#11-set-operations)
12. [Database Keys and Constraints](#12-database-keys-and-constraints)
13. [Database Normalization](#13-database-normalization)
14. [Views](#14-views)
15. [Indexes and Query Optimization](#15-indexes-and-query-optimization)
16. [Stored Procedures and Variables](#16-stored-procedures-and-variables)
17. [Triggers and Error Handling](#17-triggers-and-error-handling)
18. [Transactions and ACID Properties](#18-transactions-and-acid-properties)
19. [Date Functions and Conditional Queries](#19-date-functions-and-conditional-queries)
20. [Database Testing](#20-database-testing)
21. [ETL and Data Transformation](#21-etl-and-data-transformation)
22. [SQL Interview Practice Problems](#22-sql-interview-practice-problems)
23. [Additional Database Concepts to Learn](#23-additional-database-concepts-to-learn)
24. [Practice Strategy](#24-practice-strategy)

---

## 1. Database Fundamentals

- What is a database?
- What is a Database Management System (DBMS)?
- What is a Relational Database Management System (RDBMS)?
- Database, table, row, column, schema, and record
- MySQL and Microsoft SQL Server fundamentals
- Relational tables and relationships
- Database creation and management
- Data types and data storage
- Database integrity and consistency
- Introduction to database architecture
- Introduction to database security and recovery

## 2. SQL Command Categories

### Data Definition Language (DDL)

Used to create and modify database structures.

- `CREATE`
- `ALTER`
- `DROP`
- `TRUNCATE`

### Data Manipulation Language (DML)

Used to retrieve and modify data.

- `SELECT`
- `INSERT`
- `UPDATE`
- `DELETE`

### Data Control Language (DCL)

Used to manage database permissions.

- `GRANT`
- `REVOKE`

### Transaction Control Language (TCL)

Used to control transactions.

- `COMMIT`
- `ROLLBACK`
- `SAVEPOINT`

*Note: Some learning resources classify `SELECT` separately as Data Query Language (DQL).*

## 3. Database and Table Operations

- Creating a database
- Selecting a database
- Creating tables
- Inserting records
- Retrieving table data
- Updating existing records
- Deleting selected records
- Altering table structures
- Adding and modifying columns
- Dropping tables
- Truncating tables
- Using auto-increment and identity columns
- Understanding database schemas
- Working with sample datasets

## 4. SQL Data Retrieval and Filtering

- Selecting all columns using `SELECT *`
- Selecting specific columns
- Retrieving unique records using `DISTINCT`
- Filtering data using `WHERE`
- Sorting records using `ORDER BY`
- Sorting in ascending and descending order
- Sorting using multiple columns
- Limiting query results
- Filtering records using multiple conditions
- Filtering employee, customer, and transaction records

Example:

```sql
SELECT employee_id, employee_name, salary
FROM employees
WHERE salary > 50000
ORDER BY salary DESC;
```

## 5. SQL Operators and Conditional Logic

### Comparison Operators

- `=`
- `<>`
- `!=`
- `>`
- `<`
- `>=`
- `<=`

### Logical Operators

- `AND`
- `OR`
- `NOT`

### Other Operators and Conditions

- `LIKE`
- `IN`
- `NOT IN`
- `BETWEEN`
- `IS NULL`
- `IS NOT NULL`
- Wildcard matching using `%` and `_`
- Combining multiple filtering conditions
- Using parentheses to control condition evaluation
- Conditional categorization using `CASE`

Example:

```sql
SELECT employee_name, salary
FROM employees
WHERE salary BETWEEN 30000 AND 80000
  AND department_id IN (1, 2, 3);
```

## 6. SQL Aggregate Functions

Aggregate functions calculate summary values from multiple records.

- `COUNT()`
- `SUM()`
- `AVG()`
- `MIN()`
- `MAX()`

Practice exercises:

- Count the total number of employees.
- Calculate the total salary.
- Calculate the average salary.
- Find the minimum salary.
- Find the maximum salary.
- Count customers by category.
- Calculate totals using related tables.
- Apply aggregate functions with filters.

Example:

```sql
SELECT
    COUNT(*) AS total_employees,
    AVG(salary) AS average_salary,
    MAX(salary) AS highest_salary
FROM employees;
```

## 7. GROUP BY and HAVING

- Grouping records using `GROUP BY`
- Grouping by one column
- Grouping by multiple columns
- Counting records in each group
- Calculating total and average salary by department
- Filtering individual records with `WHERE`
- Filtering aggregated groups with `HAVING`
- Using aggregate functions with joins
- Finding departments based on employee counts
- Finding groups that satisfy business conditions

Example:

```sql
SELECT department_id, COUNT(*) AS employee_count
FROM employees
GROUP BY department_id
HAVING COUNT(*) >= 5;
```

## 8. SQL Joins

Joins combine data from related tables.

- `INNER JOIN`
- `LEFT JOIN`
- `RIGHT JOIN`
- `FULL OUTER JOIN`
- `CROSS JOIN`
- `SELF JOIN`
- `NATURAL JOIN`
- Joining three or more tables
- Joining tables using primary and foreign keys
- Using table aliases
- Understanding join conditions
- Identifying unmatched records
- Finding records without related entries
- Combining employee and department information
- Combining customers, orders, and order items
- Database-specific alternatives for unsupported join types

Example:

```sql
SELECT
    e.employee_name,
    d.department_name
FROM employees AS e
INNER JOIN departments AS d
    ON e.department_id = d.department_id;
```

## 9. Subqueries

A subquery is a query nested inside another SQL query.

Topics:

- Single-row subqueries
- Multiple-row subqueries
- Nested subqueries
- Correlated subqueries
- Subqueries in `WHERE`
- Subqueries in `FROM`
- Subqueries with aggregate functions
- Subqueries using `IN`
- Subqueries using `EXISTS`
- Subqueries using `ALL` and `ANY`
- Comparing salaries against average salaries
- Finding employees above department averages
- Finding minimum and maximum values within groups
- Solving second-highest-salary problems

Example:

```sql
SELECT employee_name, salary
FROM employees
WHERE salary > (
    SELECT AVG(salary)
    FROM employees
);
```

## 10. Common Table Expressions

- Understanding Common Table Expressions (CTEs)
- Creating CTEs using `WITH`
- Retrieving data from a CTE
- Filtering data through CTEs
- Aggregating data through CTEs
- Using multiple CTEs
- Organizing complex SQL queries
- Comparing CTEs with subqueries
- Introduction to recursive CTEs

Example:

```sql
WITH DepartmentSalary AS (
    SELECT department_id, AVG(salary) AS avg_salary
    FROM employees
    GROUP BY department_id
)
SELECT *
FROM DepartmentSalary
WHERE avg_salary > 50000;
```

## 11. Set Operations

- `UNION`
- `UNION ALL`
- Combining results from multiple queries
- Understanding duplicate handling
- Matching column counts and compatible data types
- Comparing results from different tables
- Introduction to `INTERSECT` and `EXCEPT` where supported

Example:

```sql
SELECT city FROM customers
UNION
SELECT city FROM suppliers;
```

## 12. Database Keys and Constraints

### Database Keys

- Primary key
- Foreign key
- Candidate key
- Composite key
- Unique key
- Alternate key
- Super key
- Relationships between parent and child tables

### Constraints

- `PRIMARY KEY`
- `FOREIGN KEY`
- `UNIQUE`
- `NOT NULL`
- `CHECK`
- `DEFAULT`
- Auto-increment and identity columns
- Referential integrity
- Preventing duplicate records
- Preventing invalid values
- Handling constraint violations
- Understanding delete and update behavior for related records

## 13. Database Normalization

- Understanding database redundancy
- Understanding update, insertion, and deletion anomalies
- First Normal Form (1NF)
- Second Normal Form (2NF)
- Third Normal Form (3NF)
- Functional dependencies
- Partial dependencies
- Transitive dependencies
- Introduction to denormalization
- Designing related tables
- Maintaining data consistency

## 14. Views

- Understanding database views
- Creating views using `CREATE VIEW`
- Retrieving data from views
- Creating views with selected columns
- Creating views with filtering conditions
- Joining multiple tables in a view
- Using aggregate functions in views
- Updating views where supported
- Dropping and recreating views
- Understanding view limitations
- Introduction to materialized or indexed views, depending on the database

Example:

```sql
CREATE VIEW EmployeeDetails AS
SELECT employee_id, employee_name, department_id
FROM employees;
```

## 15. Indexes and Query Optimization

### Indexes

- Understanding database indexes
- Creating indexes
- Single-column indexes
- Composite indexes
- Unique indexes
- Indexes on frequently filtered columns
- Indexes on join columns
- Dropping indexes
- Understanding index maintenance overhead

### Query Optimization

- Understanding query performance
- Identifying inefficient queries
- Understanding execution plans
- Introduction to `EXPLAIN` in MySQL
- Introduction to execution plans in SQL Server
- Understanding table scans and index access
- Measuring query performance before and after changes
- Understanding the trade-off between read performance and write overhead

## 16. Stored Procedures and Variables

- Understanding stored procedures
- Creating stored procedures
- Executing stored procedures
- Declaring SQL variables
- Assigning values to variables
- Passing input parameters
- Returning query results
- Validating input parameters
- Handling duplicate identifiers
- Implementing database business rules
- Testing procedure outputs and database changes
- Understanding differences between MySQL and SQL Server procedure syntax

## 17. Triggers and Error Handling

### Triggers

- Understanding database triggers
- Creating triggers
- Triggers on insert, update, and delete operations
- Validating records through triggers
- Recording changes to database records
- Auditing data modifications
- Testing trigger execution
- Understanding trigger side effects
- Handling multi-row operations in SQL Server triggers

### Error Handling

- Handling database errors
- Using `TRY...CATCH` in SQL Server
- Using `ERROR_MESSAGE()`
- Raising errors using `RAISERROR`
- Handling duplicate-key errors
- Testing invalid inputs
- Validating failed database operations
- Understanding database-specific error-handling syntax

## 18. Transactions and ACID Properties

### Transaction Commands

- `BEGIN TRANSACTION`
- `START TRANSACTION`
- `COMMIT`
- `ROLLBACK`
- `SAVEPOINT`
- Rolling back to a savepoint

### ACID Properties

- Atomicity
- Consistency
- Isolation
- Durability

### Practical Transaction Scenarios

- Updating related records in a single transaction
- Testing successful transactions
- Testing rollback scenarios
- Maintaining data consistency
- Transferring money between accounts
- Updating balances across multiple accounts
- Checking transaction failures
- Understanding transaction boundaries
- Introduction to concurrency and isolation levels

## 19. Date Functions and Conditional Queries

- Filtering records by date
- Finding employees who joined during a given period
- Comparing dates
- Extracting date components
- Using date-based business conditions
- Categorizing records using `CASE`
- Applying multiple conditions
- Handling missing dates
- Validating date ranges
- Understanding date-function differences between database products

## 20. Database Testing

Database testing validates that stored data, database rules, and database operations behave as expected.

### Functional Database Testing

- Verifying insert, update, and delete operations
- Checking stored procedure results
- Validating trigger execution
- Verifying database business rules

### Data Integrity Testing

- Validating primary key constraints
- Validating foreign key relationships
- Checking duplicate records
- Finding missing records
- Identifying orphan records
- Checking null values
- Validating required fields
- Verifying record counts

### Data Validation Testing

- Comparing expected and actual database values
- Validating calculated values
- Checking data types and formats
- Verifying data consistency across related tables
- Checking data after UI or API operations
- Validating data migration results

### Other Database Testing Areas

- Database performance testing
- Database security testing
- SQL injection testing fundamentals
- Stored procedure testing
- Trigger testing
- Data migration testing
- Database regression testing

### Example: Finding Records Without a Matching Department

```sql
SELECT e.employee_id, e.employee_name
FROM employees AS e
LEFT JOIN departments AS d
    ON e.department_id = d.department_id
WHERE d.department_id IS NULL;
```

This query can help identify employees whose department reference has no matching department record. Whether such records are invalid depends on the database rules and whether a null department is permitted.

## 21. ETL and Data Transformation

ETL stands for Extract, Transform, and Load.

- Understanding ETL fundamentals
- Extracting data from a source
- Transforming data into the required format
- Loading data into a target database
- Source-to-target data validation
- Comparing source and target record counts
- Checking duplicate records
- Checking missing records
- Validating transformed values
- Verifying data consistency after loading
- Introduction to Pentaho Data Integration
- Understanding transformation files
- Testing data transformation workflows

## 22. SQL Interview Practice Problems

Practice writing queries for the following problems:

1. Find the second-highest salary.
2. Find the highest-paid employee in each department.
3. Find employees earning above the overall average salary.
4. Find employees earning above their department's average salary.
5. Find departments with more than a specified number of employees.
6. Find the minimum and maximum salary in each department.
7. Find employees who do not have a matching department.
8. Find customers who have never placed an order.
9. Find duplicate records in a table.
10. Find records that exist in one table but not another.
11. Calculate the total salary by department.
12. Calculate order totals using quantities and unit prices.
13. Retrieve the latest records using date-based conditions.
14. Combine information from multiple related tables.
15. Solve join problems involving three or more tables.
16. Use subqueries to compare individual records with aggregate results.
17. Use `GROUP BY` and `HAVING` to identify qualifying groups.
18. Use `UNION` and `UNION ALL` to combine query results.
19. Validate data consistency between related tables.
20. Write queries to verify database business rules.

## 23. Additional Database Concepts to Learn

The following topics are useful extensions to the current practice material. Their inclusion in this roadmap does not mean that every topic is already implemented in the practice files.

### Advanced SQL

- Window functions: `ROW_NUMBER()`, `RANK()`, and `DENSE_RANK()`
- `LAG()` and `LEAD()`
- Running totals and cumulative aggregates
- Recursive CTEs
- Advanced date and string functions
- Query execution-plan analysis

### Database Design and Architecture

- Entity-Relationship (ER) diagrams
- One-to-one, one-to-many, and many-to-many relationships
- Database schema design
- Normalization and denormalization trade-offs
- OLTP versus OLAP
- Data warehousing fundamentals

### Transactions and Concurrency

- Isolation levels
- Dirty reads, non-repeatable reads, and phantom reads
- Deadlocks and locking
- Optimistic and pessimistic concurrency
- Transaction recovery

### Administration and Reliability

- Database backup and restore exercises
- Point-in-time recovery
- Database users, roles, and permissions
- Least-privilege access
- Monitoring and performance diagnostics
- Database migration and version control

### Automation and QA Integration

- Connecting Python to a database
- Running SQL queries from Python
- Validating database results with automated tests
- Integrating database checks into Playwright tests
- Automating source-to-target comparisons
- Generating test data
- Integrating database validation into CI/CD pipelines

## 24. Practice Strategy

Use the following sequence to build practical SQL skills:

1. Learn database fundamentals and table creation.
2. Practice `SELECT`, filtering, sorting, inserts, updates, and deletes.
3. Solve problems using aggregate functions and grouping.
4. Practice joins using multiple related tables.
5. Solve subquery and CTE problems.
6. Practice keys, constraints, and data validation.
7. Learn views, indexes, procedures, and triggers.
8. Practice transaction scenarios and rollback behavior.
9. Solve database testing problems using expected and actual results.
10. Practice ETL source-to-target validation.
11. Solve SQL interview problems without looking at the solution.
12. Connect Python automation to a database and validate results.

### Repository Practice Guidelines

- Maintain separate SQL scripts for different topics.
- Use meaningful table and column names.
- Include sample data and expected query results.
- Test both valid and invalid scenarios.
- Keep MySQL and SQL Server syntax clearly separated when they differ.
- Document any assumptions behind business rules.
- Add new exercises as you complete them.
- Mark a topic as completed only after you have implemented and tested it.

---

**Goal:** Build a practical understanding of SQL querying, database design, data validation, database testing, and automation so these skills can be applied to real-world software development and QA engineering.
