# Database Fundamentals: Practical Examples for MySQL and SQL Server

This README covers 10 core database topics with beginner-friendly explanations and practical bookstore examples. SQL code is provided for both MySQL and Microsoft SQL Server, with dialect-specific versions where syntax differs.

## Table of Contents

- [SQL Command Families: DDL, DML, and TCL](#sql-command-families-ddl-dml-and-tcl)
- [1. DBMS Fundamentals](#1-dbms-fundamentals)
- [2. SQL: SELECT, WHERE, and ORDER BY](#2-sql-select-where-and-order-by)
- [3. SQL Joins and Subqueries](#3-joins-and-subqueries)
- [4. Aggregate Functions and GROUP BY](#4-aggregate-functions-and-group-by)
- [5. Primary and Foreign Keys](#5-primary-and-foreign-keys)
- [6. Normalization: 1NF, 2NF, and 3NF](#6-normalization-1nf-2nf-and-3nf)
- [7. Indexes and Query Optimization](#7-indexes-and-query-optimization)
- [8. Transactions and ACID Properties](#8-transactions-and-acid)
- [9. Views, Stored Procedures, and Triggers](#9-views-stored-procedures-and-triggers)
- [10. Backup, Recovery, and Database Security](#10-backup-recovery-and-security)
- [Practice Exercises](#practice-exercises)
- [Key Takeaways](#key-takeaways)

### Topics covered

The command-family reference also covers DDL (`CREATE`, `ALTER`, `TRUNCATE`, `DROP`), DML (`SELECT`, `INSERT`, `UPDATE`, `DELETE`), and TCL (transactions, `COMMIT`, `ROLLBACK`, and savepoints).

1. DBMS fundamentals
2. SQL querying with `SELECT`, `WHERE`, and `ORDER BY`
3. SQL joins and subqueries
4. Aggregate functions and `GROUP BY`
5. Primary keys and foreign keys
6. Normalization: first, second, and third normal forms (1NF, 2NF, and 3NF)
7. Indexes and query optimization
8. Transactions and ACID properties
9. Views, stored procedures, and triggers
10. Backup, recovery, and database security

This guide explains core database topics using an online bookstore as a running example. SQL examples are labeled **MySQL** or **Microsoft SQL Server**. The examples target MySQL 8.x and modern Microsoft SQL Server versions. Some details, especially permissions, collations, backup configuration, and administrative features, depend on server version and setup.

## Example database

The bookstore tracks customers, books, orders, and the books in each order. Run the matching setup script for your database product; the syntax is intentionally separate where the systems differ.

### MySQL setup

```sql
CREATE DATABASE bookstore;
USE bookstore;

CREATE TABLE Customers (
    CustomerID INT NOT NULL AUTO_INCREMENT,
    FullName   VARCHAR(200) NOT NULL,
    Email      VARCHAR(255) NOT NULL,
    City       VARCHAR(100) NULL,
    PRIMARY KEY (CustomerID),
    UNIQUE (Email)
) ENGINE = InnoDB;

CREATE TABLE Books (
    BookID INT NOT NULL AUTO_INCREMENT,
    Title  VARCHAR(250) NOT NULL,
    Author VARCHAR(200) NOT NULL,
    Price  DECIMAL(10, 2) NOT NULL,
    Stock  INT NOT NULL DEFAULT 0,
    PRIMARY KEY (BookID),
    CHECK (Price >= 0),
    CHECK (Stock >= 0)
) ENGINE = InnoDB;

CREATE TABLE Orders (
    OrderID    INT NOT NULL AUTO_INCREMENT,
    CustomerID INT NOT NULL,
    OrderDate  DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
    Status     VARCHAR(30) NOT NULL DEFAULT 'pending',
    PRIMARY KEY (OrderID),
    FOREIGN KEY (CustomerID) REFERENCES Customers(CustomerID)
) ENGINE = InnoDB;

CREATE TABLE OrderItems (
    OrderID   INT NOT NULL,
    BookID    INT NOT NULL,
    Quantity  INT NOT NULL,
    UnitPrice DECIMAL(10, 2) NOT NULL,
    PRIMARY KEY (OrderID, BookID),
    FOREIGN KEY (OrderID) REFERENCES Orders(OrderID),
    FOREIGN KEY (BookID) REFERENCES Books(BookID),
    CHECK (Quantity > 0),
    CHECK (UnitPrice >= 0)
) ENGINE = InnoDB;
```

MySQL uses `AUTO_INCREMENT` for generated integer keys. Use InnoDB for transactions and foreign-key enforcement. MySQL versions before 8.0.16 parsed `CHECK` constraints but did not enforce them; use a current supported release if relying on checks.

### Microsoft SQL Server setup

```sql
CREATE DATABASE bookstore;
GO
USE bookstore;
GO

CREATE TABLE dbo.Customers (
    CustomerID INT IDENTITY(1,1) NOT NULL PRIMARY KEY,
    FullName   NVARCHAR(200) NOT NULL,
    Email      NVARCHAR(255) NOT NULL UNIQUE,
    City       NVARCHAR(100) NULL
);

CREATE TABLE dbo.Books (
    BookID INT IDENTITY(1,1) NOT NULL PRIMARY KEY,
    Title  NVARCHAR(250) NOT NULL,
    Author NVARCHAR(200) NOT NULL,
    Price  DECIMAL(10, 2) NOT NULL CHECK (Price >= 0),
    Stock  INT NOT NULL DEFAULT 0 CHECK (Stock >= 0)
);

CREATE TABLE dbo.Orders (
    OrderID    INT IDENTITY(1,1) NOT NULL PRIMARY KEY,
    CustomerID INT NOT NULL,
    OrderDate  DATETIME2 NOT NULL DEFAULT SYSUTCDATETIME(),
    Status     NVARCHAR(30) NOT NULL DEFAULT N'pending',
    FOREIGN KEY (CustomerID) REFERENCES dbo.Customers(CustomerID)
);

CREATE TABLE dbo.OrderItems (
    OrderID   INT NOT NULL,
    BookID    INT NOT NULL,
    Quantity  INT NOT NULL CHECK (Quantity > 0),
    UnitPrice DECIMAL(10, 2) NOT NULL CHECK (UnitPrice >= 0),
    PRIMARY KEY (OrderID, BookID),
    FOREIGN KEY (OrderID) REFERENCES dbo.Orders(OrderID),
    FOREIGN KEY (BookID) REFERENCES dbo.Books(BookID)
);
GO
```

SQL Server uses `IDENTITY(1,1)` for generated integer keys and `NVARCHAR` for Unicode text. `GO` is a batch separator recognized by tools such as SQL Server Management Studio and `sqlcmd`; it is not itself a Transact-SQL statement.

## SQL command families: DDL, DML, and TCL

SQL commands are often grouped by what they do. The exact classification of a few commands can vary between textbooks and products; in this guide, `SELECT` is grouped with DML because it reads data, though some sources call it DQL (Data Query Language).

| Category | Purpose | Common commands | Topics in this guide |
|---|---|---|---|
| DDL (Data Definition Language) | Define or change database objects and structure | `CREATE`, `ALTER`, `DROP`, `TRUNCATE` | Example schema, keys, views, indexes, procedures, triggers |
| DML (Data Manipulation Language) | Read and change table data | `SELECT`, `INSERT`, `UPDATE`, `DELETE` | Filtering, joins, aggregates, keys, transactions |
| TCL (Transaction Control Language) | Control transaction boundaries and recovery of uncommitted work | `START TRANSACTION` / `BEGIN TRANSACTION`, `COMMIT`, `ROLLBACK`, savepoints | Transactions and ACID |

### DDL: create, alter, truncate, and drop

DDL changes database objects. Run the appropriate dialect version. Destructive commands such as `DROP` and `TRUNCATE` remove data or objects; use them only when intended.

**CREATE** builds a database object. The earlier setup section uses `CREATE DATABASE` and `CREATE TABLE`. Here is an example adding an index:

```sql
CREATE INDEX IX_Orders_CustomerID_OrderDate
ON Orders (CustomerID, OrderDate);
```

The syntax works in MySQL and SQL Server. In SQL Server, prefix the table with its schema if needed, for example `dbo.Orders`.

**ALTER** changes an existing object's structure.

MySQL:

```sql
ALTER TABLE Books
ADD COLUMN Publisher VARCHAR(200) NULL;
```

SQL Server:

```sql
ALTER TABLE dbo.Books
ADD Publisher NVARCHAR(200) NULL;
```

**TRUNCATE TABLE** removes all rows while retaining the table definition. It does not support a `WHERE` clause. Foreign-key relationships, permissions, identity reset behavior, and transaction details differ by product and context; a referenced table may not be truncatable.

MySQL:

```sql
TRUNCATE TABLE OrderItems;
```

SQL Server:

```sql
TRUNCATE TABLE dbo.OrderItems;
```

**DROP** removes a database object itself, including its definition. For example, remove an unused index with `DROP INDEX`; syntax differs:

MySQL:

```sql
DROP INDEX IX_Orders_CustomerID_OrderDate ON Orders;
```

SQL Server:

```sql
DROP INDEX IX_Orders_CustomerID_OrderDate ON dbo.Orders;
```

To remove a table, use `DROP TABLE table_name` (with `dbo.` in SQL Server where appropriate). Dropping a table removes its data and definition. Check dependencies first.

### DML: select and change data

DML works with rows. The examples below use the bookstore tables from this guide.

**SELECT** reads rows and can filter, join, group, and sort them. These are explained in the SELECT, joins, and aggregate sections. For example:

```sql
SELECT BookID, Title, Price
FROM Books
WHERE Stock > 0
ORDER BY Price ASC;
```

**INSERT** adds rows. This example works in both systems:

```sql
INSERT INTO Books (Title, Author, Price, Stock)
VALUES ('Practical Databases', 'A. Example', 24.99, 12);
```

Capture the generated ID with product-specific syntax when the application needs it.

MySQL:

```sql
INSERT INTO Books (Title, Author, Price, Stock)
VALUES ('SQL in Practice', 'B. Example', 19.50, 8);

SELECT LAST_INSERT_ID() AS NewBookID;
```

SQL Server:

```sql
INSERT INTO dbo.Books (Title, Author, Price, Stock)
OUTPUT INSERTED.BookID AS NewBookID
VALUES (N'SQL in Practice', N'B. Example', 19.50, 8);
```

**UPDATE** changes existing rows. Use a `WHERE` clause to target the intended records; without it, every row is updated.

```sql
UPDATE Books
SET Price = 21.99
WHERE BookID = 1;
```

**DELETE** removes selected rows. Check the `WHERE` clause carefully; without it, every row is deleted.

```sql
DELETE FROM Books
WHERE BookID = 1;
```

A foreign key may prevent deleting a book that appears in `OrderItems`. This preserves referential integrity; the business might instead mark the book as inactive.

### TCL: control transactions

TCL statements control whether a group of changes becomes permanent. The exact begin syntax differs across MySQL and SQL Server.

**MySQL transaction with a savepoint:**

```sql
START TRANSACTION;

UPDATE Books
SET Stock = Stock - 1
WHERE BookID = 1 AND Stock > 0;

SAVEPOINT after_stock_update;

-- Add related order and order-item rows here.
-- If a later step fails but the transaction can continue:
-- ROLLBACK TO SAVEPOINT after_stock_update;

COMMIT;
-- If the whole operation must be canceled, use ROLLBACK instead.
```

**SQL Server transaction with a savepoint:**

```sql
BEGIN TRANSACTION;

UPDATE dbo.Books
SET Stock = Stock - 1
WHERE BookID = 1 AND Stock > 0;

SAVE TRANSACTION after_stock_update;

-- Add related order and order-item rows here.
-- If a later step fails but the transaction can continue:
-- ROLLBACK TRANSACTION after_stock_update;

COMMIT TRANSACTION;
-- If the whole operation must be canceled, use ROLLBACK TRANSACTION instead.
```

A savepoint allows a transaction to roll back part of its work while keeping the earlier part active. Savepoint behavior and transaction error handling have product-specific details. Applications should check that the stock update affected one row and should handle failures explicitly. Keep transactions short so they do not hold locks longer than necessary.

### How these command families connect to the topics

- Use DDL to create tables and enforce primary keys, foreign keys, checks, and indexes.
- Use DML to query with `SELECT`, joins, subqueries, and aggregates, and to add or change bookstore data with `INSERT`, `UPDATE`, and `DELETE`.
- Use TCL to keep related changes together, such as creating an order and reducing inventory, supporting transaction integrity and ACID behavior.
- Views, stored procedures, and triggers are database objects created with DDL; they contain or react to SQL operations.
- Backup and recovery planning protects the data changed by DML and committed by transactions.
## 1. DBMS fundamentals

A **database** is an organized collection of data. A **database management system (DBMS)** is software used to define, store, query, update, and protect that data. MySQL and Microsoft SQL Server are relational DBMSs: they organize information into tables with rows and columns.

A DBMS commonly provides:

- **Data definition:** create tables, columns, indexes, and constraints.
- **Data manipulation:** insert, read, update, and delete records.
- **Query processing:** parse SQL and choose an execution strategy.
- **Concurrency control:** coordinate simultaneous requests from different users.
- **Integrity enforcement:** reject invalid data, such as a negative price.
- **Access control:** grant different permissions to applications and people.
- **Recovery:** restore data after failures, using configured logs and backups.

For example, a spreadsheet-based bookstore may duplicate customer details across order sheets and develop inconsistent records. A database lets orders refer to a single customer record and can reject orders for nonexistent customers. Sound design, permissions, and backups are still needed.

## 2. SQL: SELECT, WHERE, and ORDER BY

These queries work in both systems:

```sql
SELECT Title, Author, Price
FROM Books
WHERE Price < 20
ORDER BY Price ASC, Title ASC;
```

`SELECT` chooses columns, `WHERE` filters rows, and `ORDER BY` sorts them. `ASC` is ascending and `DESC` descending. Add a tie-breaker column when predictable ordering matters.

Other comparisons include `=`, `<>`, `>`, `>=`, `<`, `<=`, and `BETWEEN`:

```sql
SELECT Title, Price
FROM Books
WHERE Price BETWEEN 10 AND 25
  AND Stock > 0;
```

Pattern matching uses `LIKE` in both systems:

```sql
SELECT Title
FROM Books
WHERE Title LIKE 'The%';
```

Whether matching is case-sensitive depends on the column or database collation. MySQL and SQL Server commonly use case-insensitive collations, but this is configurable. To match missing values, use `IS NULL` or `IS NOT NULL`, never `= NULL`:

```sql
SELECT FullName
FROM Customers
WHERE City IS NULL;
```

A storefront could use these queries to show in-stock titles or sort prices for customers.

## 3. Joins and subqueries

A join combines related rows. An `INNER JOIN` returns only rows with matches on both sides:

```sql
SELECT O.OrderID, C.FullName, O.OrderDate, O.Status
FROM Orders AS O
INNER JOIN Customers AS C
    ON C.CustomerID = O.CustomerID;
```

A `LEFT JOIN` preserves every row from the left table, even when the right table has no match:

```sql
SELECT C.CustomerID, C.FullName, O.OrderID
FROM Customers AS C
LEFT JOIN Orders AS O
    ON O.CustomerID = C.CustomerID;
```

Find customers without orders:

```sql
SELECT C.CustomerID, C.FullName
FROM Customers AS C
LEFT JOIN Orders AS O
    ON O.CustomerID = C.CustomerID
WHERE O.OrderID IS NULL;
```

Join several tables to show order lines:

```sql
SELECT O.OrderID, C.FullName, B.Title,
       I.Quantity, I.UnitPrice
FROM Orders AS O
JOIN Customers AS C ON C.CustomerID = O.CustomerID
JOIN OrderItems AS I ON I.OrderID = O.OrderID
JOIN Books AS B ON B.BookID = I.BookID
ORDER BY O.OrderID, B.Title;
```

A **subquery** is a query inside another query. This example finds books priced above the average:

```sql
SELECT Title, Price
FROM Books
WHERE Price > (SELECT AVG(Price) FROM Books);
```

`EXISTS` tests whether related data exists:

```sql
SELECT C.CustomerID, C.FullName
FROM Customers AS C
WHERE EXISTS (
    SELECT 1
    FROM Orders AS O
    WHERE O.CustomerID = C.CustomerID
);
```

Correct join conditions matter. Omitting one can multiply rows and produce misleading totals.

## 4. Aggregate functions and GROUP BY

Aggregate functions summarize rows: `COUNT` counts, `SUM` adds, `AVG` averages, and `MIN`/`MAX` find the smallest/largest value.

Count orders by status:

```sql
SELECT Status, COUNT(*) AS OrderCount
FROM Orders
GROUP BY Status
ORDER BY OrderCount DESC;
```

Calculate every order's total from its lines:

```sql
SELECT O.OrderID,
       SUM(I.Quantity * I.UnitPrice) AS OrderTotal
FROM Orders AS O
JOIN OrderItems AS I ON I.OrderID = O.OrderID
GROUP BY O.OrderID;
```

`WHERE` filters rows before grouping; `HAVING` filters groups after aggregation:

```sql
SELECT CustomerID, COUNT(*) AS OrderCount
FROM Orders
GROUP BY CustomerID
HAVING COUNT(*) >= 3;
```

The bookstore could use aggregates for monthly sales, order volume by status, or repeat-customer reports.

## 5. Primary and foreign keys

A **primary key** uniquely identifies each row. In this schema `CustomerID`, `BookID`, and `OrderID` are primary keys. `OrderItems` uses a composite key `(OrderID, BookID)`, so one book appears at most once per order; quantity represents multiple copies.

A **foreign key** requires a value to match a row in another table. `Orders.CustomerID` references `Customers.CustomerID`, so the database rejects an order for a nonexistent customer. The same principle links order items to orders and books.

When a referenced row is deleted, the database can reject the deletion or follow a rule such as cascading it. Choose actions carefully; historic orders often need to remain available even if a customer account is closed.

## 6. Normalization: 1NF, 2NF, and 3NF

**Normalization** organizes data to reduce duplication and prevent update anomalies.

- **First Normal Form (1NF):** Store one value per field. Do not store a comma-separated list of book IDs in one order row. Put one row per book in `OrderItems` instead.
- **Second Normal Form (2NF):** Be in 1NF and have every non-key column depend on the whole key. With a composite `(OrderID, BookID)` key, a customer name depends on the order, not on the entire order/book pair. Keep the name in `Customers` and the customer reference in `Orders`.
- **Third Normal Form (3NF):** Be in 2NF and have non-key columns depend on the key, not on another non-key column. If a postal code determines a city, storing city repeatedly for each customer can lead to inconsistencies. A separate postal-code table may help, depending on the actual address rules.

Normalization is a design guide. Some systems deliberately duplicate data for reporting or performance, but the duplication must be maintained consistently.

## 7. Indexes and query optimization

An **index** is an additional data structure that can help the database find rows faster. It uses storage and adds work to inserts, updates, and deletes, so create indexes to support real query patterns.

The following syntax works in both systems:

```sql
CREATE INDEX IX_Orders_CustomerID_OrderDate
ON Orders (CustomerID, OrderDate);
```

It may help this query, which finds a customer's recent orders:

```sql
SELECT OrderID, OrderDate, Status
FROM Orders
WHERE CustomerID = 42
ORDER BY OrderDate DESC;
```

Multi-column index order matters. `(CustomerID, OrderDate)` is useful when filtering by customer and then by date; it is not automatically a good index for filtering only by date. Primary keys and unique constraints also create indexes in both systems.

To investigate a slow query:

1. Measure and identify the slow query.
2. Inspect its execution plan and row estimates.
3. Confirm filters and joins use the intended columns.
4. Consider an index and its storage/write cost.
5. Measure again with representative data.

In MySQL, `EXPLAIN SELECT ...` displays the plan. In SQL Server Management Studio, use **Include Actual Execution Plan**; `SET STATISTICS IO, TIME ON` can report reads and timing. SQL Server tools and MySQL's plan commands have different output and capabilities. An index does not guarantee improvement; a scan can be cheaper for small tables or queries returning many rows.

## 8. Transactions and ACID

A **transaction** groups related operations. A checkout should record its order and line items and reduce stock consistently. If a required step fails, the application should roll back all work. The example below illustrates the transaction boundary; a real application must check affected-row counts and handle errors before committing.

### MySQL

```sql
START TRANSACTION;

UPDATE Books
SET Stock = Stock - 1
WHERE BookID = 10 AND Stock > 0;

-- Check that exactly one row was updated.
-- Insert the order and order item in this same transaction.

COMMIT;
-- If a required step fails, issue ROLLBACK instead.
```

### Microsoft SQL Server

```sql
BEGIN TRANSACTION;

UPDATE dbo.Books
SET Stock = Stock - 1
WHERE BookID = 10 AND Stock > 0;

-- Check that exactly one row was updated.
-- Insert the order and order item in this same transaction.

COMMIT TRANSACTION;
-- If a required step fails, issue ROLLBACK TRANSACTION instead.
```

ACID describes important transaction guarantees:

- **Atomicity:** a transaction is treated as a unit; it commits or its changes roll back.
- **Consistency:** constraints and business rules should hold after successful work.
- **Isolation:** concurrent work is controlled according to the configured isolation level.
- **Durability:** committed work is designed to survive failure, subject to database and storage configuration.

ACID does not replace correct business logic. For example, checking that stock is still positive during the update helps prevent selling the final copy twice.

## 9. Views, stored procedures, and triggers

### Views

A **view** is a named query. This view reports order totals and works in both systems (add `dbo.` before object names in SQL Server if using the schema shown above):

```sql
CREATE VIEW OrderSummary AS
SELECT O.OrderID, O.OrderDate, O.Status,
       C.FullName AS CustomerName,
       SUM(I.Quantity * I.UnitPrice) AS OrderTotal
FROM Orders AS O
JOIN Customers AS C ON C.CustomerID = O.CustomerID
JOIN OrderItems AS I ON I.OrderID = O.OrderID
GROUP BY O.OrderID, O.OrderDate, O.Status, C.FullName;
```

Query it like a table:

```sql
SELECT OrderID, CustomerName, OrderTotal
FROM OrderSummary
WHERE Status = 'paid';
```

A regular view stores its query definition, not a separate result copy. SQL Server supports indexed views under specific requirements. MySQL does not provide the same built-in indexed/materialized view feature; applications commonly use summary tables or scheduled jobs when persisted summaries are needed.

### Stored procedures

A **stored procedure** is a named set of database statements. Here is a minimal example in each dialect.

**MySQL:**

```sql
DELIMITER //
CREATE PROCEDURE GetCustomerOrders(IN p_customer_id INT)
BEGIN
    SELECT OrderID, OrderDate, Status
    FROM Orders
    WHERE CustomerID = p_customer_id
    ORDER BY OrderDate DESC;
END //
DELIMITER ;

CALL GetCustomerOrders(1);
```

`DELIMITER` is a command understood by the MySQL command-line client, not a SQL statement sent to the server. GUI tools may have their own routine editor or delimiter handling.

**Microsoft SQL Server:**

```sql
CREATE OR ALTER PROCEDURE dbo.GetCustomerOrders
    @CustomerID INT
AS
BEGIN
    SET NOCOUNT ON;

    SELECT OrderID, OrderDate, Status
    FROM dbo.Orders
    WHERE CustomerID = @CustomerID
    ORDER BY OrderDate DESC;
END;
GO

EXEC dbo.GetCustomerOrders @CustomerID = 1;
```

Procedures can centralize operations and permissions, but should be documented and version-controlled like application code.

### Triggers

A **trigger** runs automatically after or instead of a configured table operation. Trigger syntax and behavior differ across these products. This example records order status changes.

First create the audit table (same logical SQL in both, with schema prefix for SQL Server):

```sql
CREATE TABLE OrderStatusAudit (
    AuditID BIGINT NOT NULL AUTO_INCREMENT PRIMARY KEY,
    OrderID INT NOT NULL,
    OldStatus VARCHAR(30) NULL,
    NewStatus VARCHAR(30) NOT NULL,
    ChangedAt DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP
);
```

For SQL Server, replace the `AuditID` definition with `AuditID BIGINT IDENTITY(1,1) NOT NULL PRIMARY KEY`, use `NVARCHAR(30)` if desired, and use `DATETIME2 NOT NULL DEFAULT SYSUTCDATETIME()` for `ChangedAt`.

**MySQL trigger:**

```sql
DELIMITER //
CREATE TRIGGER Orders_Status_Audit
AFTER UPDATE ON Orders
FOR EACH ROW
BEGIN
    IF NOT (OLD.Status <=> NEW.Status) THEN
        INSERT INTO OrderStatusAudit
            (OrderID, OldStatus, NewStatus)
        VALUES
            (NEW.OrderID, OLD.Status, NEW.Status);
    END IF;
END //
DELIMITER ;
```

MySQL's `<=>` comparison treats two `NULL` values as equal. In this schema status is non-null, but the null-safe comparison is explicit.

**Microsoft SQL Server trigger:**

```sql
CREATE OR ALTER TRIGGER dbo.Orders_Status_Audit
ON dbo.Orders
AFTER UPDATE
AS
BEGIN
    SET NOCOUNT ON;

    INSERT INTO dbo.OrderStatusAudit (OrderID, OldStatus, NewStatus)
    SELECT i.OrderID, d.Status, i.Status
    FROM inserted AS i
    JOIN deleted AS d ON d.OrderID = i.OrderID
    WHERE i.Status <> d.Status;
END;
GO
```

SQL Server triggers must handle multi-row updates; `inserted` and `deleted` are row sets, which this example does. Triggers can create hidden side effects, so document them and protect audit records from unauthorized changes.

## 10. Backup, recovery, and security

A **backup** is a recoverable copy of data. A **recovery plan** describes how to restore service and data after accidental deletion, corruption, hardware failure, or another incident.

A practical plan defines:

- What gets backed up, how often, where copies are stored, and how long they are retained.
- Who can access backups and whether they are encrypted.
- **Recovery point objective (RPO):** how much recent data the business can afford to lose.
- **Recovery time objective (RTO):** how long the service can be unavailable.
- How restore tests will be performed and checked.

MySQL provides logical dumps (for example, `mysqldump`) and physical backup options; binary logs can support point-in-time recovery when configured and retained properly. SQL Server provides full, differential, and transaction log backups; point-in-time recovery depends on recovery model and an unbroken log backup chain. Exact commands and scheduling should match the server version and operational setup. A backup that has never been restored in a test is an unverified recovery plan.

Security practices:

- Use separate accounts for applications, administrators, and reporting.
- Grant only the permissions each account needs; do not use an administrative account for routine application access.
- Keep credentials out of source code and use a secret manager.
- Restrict network access and use encrypted connections where appropriate.
- Keep database software and host systems patched through a controlled process.
- Use parameterized queries in application code to reduce SQL injection risk.
- Protect backup files and logs, which may contain sensitive data.
- Review permissions and retain only the personal data the business needs.

Permission syntax differs. For example, MySQL grants database-object privileges to an account such as `'reporting_user'@'localhost'`; SQL Server separates server logins from database users and grants permissions to the database user. Configure identities and secrets for the deployment environment instead of copying sample credentials.

## Practice exercises

1. List available books from most expensive to least expensive.
2. Find customers who have never placed an order.
3. Count orders in each status.
4. Calculate each order's total from its order items.
5. Add an index to help retrieve one customer's newest orders; explain your column order.
6. Describe what should happen when an application tries to insert an order for a nonexistent customer.
7. Choose an RPO and RTO for a small bookstore and outline how to test a restore.

## Key takeaways

- MySQL and SQL Server share relational concepts, but syntax differs for identity columns, date/time types, stored routines, triggers, permissions, plans, and backups.
- Primary and foreign keys protect identity and relationships; normalization reduces avoidable duplication.
- Indexes can speed up selected queries but cost storage and write performance.
- Transactions group related changes. Backups, tested restores, least-privilege permissions, and parameterized queries are essential operational practices.



