# MySQL Interview Preparation Checklist for SDE Roles in India

This checklist covers the MySQL topics most commonly expected in software engineering interviews, including query writing, joins, indexing, normalization, transactions, storage engines, and optimization.[cite:12][cite:13][cite:18]

## MySQL Foundations
- [ ] What MySQL is and where it is used
- [ ] RDBMS basics
- [ ] Database, table, row, column, schema
- [ ] Difference between SQL and MySQL
- [ ] MySQL architecture at a high level
- [ ] Client-server model
- [ ] Storage engine concept
- [ ] InnoDB vs MyISAM

## SQL Language Basics in MySQL
- [ ] DDL, DML, DCL, TCL, DQL
- [ ] CREATE, ALTER, DROP, TRUNCATE
- [ ] INSERT, UPDATE, DELETE
- [ ] SELECT syntax and execution flow basics
- [ ] WHERE, ORDER BY, GROUP BY, HAVING
- [ ] DISTINCT
- [ ] LIMIT and OFFSET
- [ ] Aliases
- [ ] Comments and query readability

## Data Types
- [ ] Numeric data types
- [ ] Character data types
- [ ] DATE, DATETIME, TIMESTAMP
- [ ] BOOLEAN handling in MySQL
- [ ] ENUM and SET basics
- [ ] Choosing the right data type
- [ ] NULL vs NOT NULL behavior

## Constraints and Keys
- [ ] PRIMARY KEY
- [ ] FOREIGN KEY
- [ ] UNIQUE
- [ ] NOT NULL
- [ ] DEFAULT
- [ ] CHECK support and practical note in MySQL
- [ ] Composite key
- [ ] Candidate key and alternate key
- [ ] Super key
- [ ] Referential integrity

## Query Writing Essentials
- [ ] Filtering rows with WHERE
- [ ] BETWEEN, IN, LIKE
- [ ] Wildcards in LIKE
- [ ] IS NULL and IS NOT NULL
- [ ] Sorting with ORDER BY
- [ ] Aggregates: COUNT, SUM, AVG, MIN, MAX
- [ ] GROUP BY and HAVING
- [ ] CASE expression basics
- [ ] IFNULL and COALESCE basics

## Joins
- [ ] INNER JOIN
- [ ] LEFT JOIN
- [ ] RIGHT JOIN
- [ ] SELF JOIN
- [ ] CROSS JOIN
- [ ] Natural join concept
- [ ] Join vs subquery
- [ ] When joins create duplicates
- [ ] Join conditions and performance basics

## Subqueries and Advanced Querying
- [ ] Single-row subquery
- [ ] Multi-row subquery
- [ ] Correlated subquery
- [ ] EXISTS and NOT EXISTS
- [ ] ANY, ALL, IN
- [ ] Derived tables
- [ ] Common Table Expressions if supported in target version
- [ ] Window functions basics if relevant to role/version

## Normalization and Schema Design
- [ ] 1NF, 2NF, 3NF
- [ ] BCNF overview
- [ ] Denormalization and when to use it
- [ ] Data redundancy
- [ ] Functional dependency basics
- [ ] Schema design trade-offs
- [ ] One-to-one, one-to-many, many-to-many relationships

## Indexing
- [ ] What an index is
- [ ] Why indexes improve reads
- [ ] Why indexes can slow writes
- [ ] Primary index vs secondary index
- [ ] Clustered vs non-clustered idea
- [ ] Composite index
- [ ] Covering index concept
- [ ] Unique index
- [ ] Full-text index basics
- [ ] When indexes are not used well
- [ ] Index selectivity concept

## Transactions and ACID
- [ ] What a transaction is
- [ ] ACID properties
- [ ] COMMIT, ROLLBACK, SAVEPOINT
- [ ] Auto-commit behavior
- [ ] Transaction boundaries
- [ ] Consistent state concept
- [ ] Why InnoDB matters for transactions

## Concurrency and Locking
- [ ] Shared lock and exclusive lock
- [ ] Row-level locking
- [ ] Table-level locking
- [ ] Deadlock basics in MySQL
- [ ] Isolation levels
- [ ] Dirty read
- [ ] Non-repeatable read
- [ ] Phantom read
- [ ] MVCC basics

## Views, Procedures, and Triggers
- [ ] What a view is
- [ ] Updatable vs non-updatable views
- [ ] Stored procedure basics
- [ ] Function basics
- [ ] Trigger basics
- [ ] When to use procedures or triggers
- [ ] Pros and cons in interview discussion

## Performance and Optimization
- [ ] Query optimization basics
- [ ] EXPLAIN plan basics
- [ ] Full table scan
- [ ] Slow queries and common reasons
- [ ] Avoiding SELECT * in critical paths
- [ ] Proper indexing strategy
- [ ] Join order basics
- [ ] Pagination strategies
- [ ] Batch inserts and updates

## Storage and Engine Concepts
- [ ] InnoDB key features
- [ ] MyISAM key features
- [ ] Transactions support by engine
- [ ] Foreign key support differences
- [ ] Crash recovery basics
- [ ] Buffer pool idea
- [ ] Redo log and undo log at a high level

## Backup, Recovery, and Maintenance
- [ ] Backup basics
- [ ] Logical backup vs physical backup
- [ ] Restore basics
- [ ] Replication basics
- [ ] Partitioning basics
- [ ] Archiving old data
- [ ] Maintenance commands at a high level

## MySQL Commands and Interview Practice
- [ ] Create sample schemas and write queries by hand
- [ ] Practice joins on 3-4 related tables
- [ ] Practice aggregate queries with GROUP BY and HAVING
- [ ] Practice subqueries and correlated subqueries
- [ ] Practice index-related interview explanations
- [ ] Practice transaction and isolation questions
- [ ] Practice optimization cases using EXPLAIN
- [ ] Be able to explain InnoDB vs MyISAM clearly

## High-Priority Revision Order
- [ ] SELECT, WHERE, GROUP BY, HAVING, ORDER BY
- [ ] Joins and subqueries
- [ ] Keys, constraints, and relationships
- [ ] Normalization
- [ ] Indexes
- [ ] Transactions and ACID
- [ ] Locking and isolation levels
- [ ] Views, triggers, procedures
- [ ] Query optimization and EXPLAIN

## How to Study Efficiently
- [ ] First pass: definitions and syntax basics
- [ ] Second pass: write queries manually without auto-complete
- [ ] Third pass: solve interview-style schema questions
- [ ] Fourth pass: revise optimization and transaction scenarios
- [ ] Final pass: prepare one-line differences for common comparison questions
