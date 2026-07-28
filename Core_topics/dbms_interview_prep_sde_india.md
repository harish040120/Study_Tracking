# DBMS Interview Preparation Checklist for SDE Roles in India

This checklist covers the DBMS concepts most often asked in software engineering interviews, especially around database design, normalization, keys, transactions, concurrency control, indexing, and SQL-related theory.[cite:6][cite:17][cite:20]

## DBMS Foundations
- [ ] What a DBMS is
- [ ] What an RDBMS is
- [ ] DBMS vs RDBMS
- [ ] Advantages of DBMS
- [ ] Database users and roles
- [ ] Data models overview
- [ ] Why databases are needed over file systems
- [ ] DBMS components at a high level

## Core Terminology
- [ ] Schema
- [ ] Instance
- [ ] Relation
- [ ] Tuple
- [ ] Attribute
- [ ] Degree and cardinality
- [ ] Domain
- [ ] Metadata
- [ ] Data dictionary

## Database Architecture
- [ ] Three-schema architecture
- [ ] Physical level
- [ ] Logical level
- [ ] View level
- [ ] Data abstraction
- [ ] Data independence
- [ ] Physical data independence
- [ ] Logical data independence

## ER Model and Design
- [ ] Entity
- [ ] Attribute
- [ ] Relationship
- [ ] Strong entity vs weak entity
- [ ] Simple, composite, multivalued, derived attributes
- [ ] Cardinality mapping
- [ ] Participation constraints
- [ ] ER diagram basics
- [ ] Converting ER model to relational model

## Keys and Constraints
- [ ] Super key
- [ ] Candidate key
- [ ] Primary key
- [ ] Alternate key
- [ ] Composite key
- [ ] Foreign key
- [ ] Integrity constraints
- [ ] Entity integrity
- [ ] Referential integrity
- [ ] Domain constraints

## Relational Model
- [ ] Relational schema
- [ ] Relational algebra basics
- [ ] Selection, projection, union, set difference, Cartesian product
- [ ] Join operations overview
- [ ] Relational calculus basics
- [ ] Codd's rules overview

## Normalization
- [ ] Data redundancy and anomalies
- [ ] Insertion anomaly
- [ ] Update anomaly
- [ ] Deletion anomaly
- [ ] Functional dependency
- [ ] Full functional dependency
- [ ] Partial dependency
- [ ] Transitive dependency
- [ ] 1NF
- [ ] 2NF
- [ ] 3NF
- [ ] BCNF
- [ ] 4NF overview
- [ ] Denormalization and trade-offs

## Transactions and ACID
- [ ] What a transaction is
- [ ] ACID properties
- [ ] Transaction states
- [ ] COMMIT and ROLLBACK
- [ ] Schedules in DBMS
- [ ] Serial schedule
- [ ] Concurrent schedule
- [ ] Serializable schedule
- [ ] Recoverable and non-recoverable schedules

## Concurrency Control
- [ ] Why concurrency control is needed
- [ ] Lost update problem
- [ ] Dirty read
- [ ] Unrepeatable read
- [ ] Phantom read
- [ ] Lock-based protocols
- [ ] Two-phase locking
- [ ] Shared and exclusive locks
- [ ] Timestamp-based protocols
- [ ] Validation-based protocol overview
- [ ] Deadlock in DBMS

## Recovery Management
- [ ] Failure types in DBMS
- [ ] Log-based recovery
- [ ] Write-ahead logging
- [ ] Checkpoints
- [ ] Undo and redo
- [ ] Shadow paging basics
- [ ] Crash recovery basics

## Indexing and File Organization
- [ ] What indexing is
- [ ] Primary index
- [ ] Secondary index
- [ ] Dense index
- [ ] Sparse index
- [ ] Clustered vs non-clustered index concept
- [ ] B-tree and B+ tree basics
- [ ] Hash indexing basics
- [ ] File organization overview

## SQL Concepts in DBMS Theory
- [ ] SQL as a database language
- [ ] DDL, DML, DCL, TCL
- [ ] SQL joins at a theory level
- [ ] Subqueries
- [ ] Views
- [ ] Triggers
- [ ] Stored procedures overview
- [ ] Assertions and constraints

## Database Design and Relationships
- [ ] One-to-one relationship
- [ ] One-to-many relationship
- [ ] Many-to-many relationship
- [ ] Mapping relationships into tables
- [ ] Choosing primary and foreign keys correctly
- [ ] Designing scalable schemas
- [ ] Trade-offs between normalization and performance

## Distributed and Advanced DBMS Basics
- [ ] Centralized vs distributed databases
- [ ] Fragmentation basics
- [ ] Replication basics
- [ ] CAP idea at a high level if asked
- [ ] OLTP vs OLAP
- [ ] Data warehousing basics
- [ ] NoSQL vs RDBMS at a high level

## Common Direct Interview Questions to Prepare
- [ ] Difference between DBMS and file system
- [ ] Difference between DBMS and RDBMS
- [ ] Difference between primary key and unique key
- [ ] Difference between DELETE, DROP, and TRUNCATE
- [ ] Difference between WHERE and HAVING
- [ ] Difference between clustered and non-clustered index
- [ ] Difference between 2NF, 3NF, and BCNF
- [ ] Difference between shared lock and exclusive lock
- [ ] Difference between serializable and conflict serializable
- [ ] Difference between deadlock and starvation

## Interview-Focused Practice
- [ ] Practice ER diagram questions
- [ ] Practice normalization problems step by step
- [ ] Practice keys and constraint identification
- [ ] Practice transaction schedule questions
- [ ] Practice serializability basics
- [ ] Practice indexing explanation with examples
- [ ] Practice SQL-linked DBMS theory questions
- [ ] Revise DBMS together with OS and CN for placements

## High-Priority Revision Order
- [ ] DBMS vs RDBMS and architecture
- [ ] Keys and constraints
- [ ] ER model and relationships
- [ ] Normalization and dependencies
- [ ] Transactions and ACID
- [ ] Concurrency control and serializability
- [ ] Recovery management
- [ ] Indexing and B+ trees
- [ ] SQL concepts linked to DBMS theory

## How to Study Efficiently
- [ ] First pass: definitions and terminology
- [ ] Second pass: ER model, keys, and normalization
- [ ] Third pass: transactions, locks, and recovery
- [ ] Fourth pass: indexing and SQL-linked theory
- [ ] Final pass: prepare short comparison answers for viva-style questions
