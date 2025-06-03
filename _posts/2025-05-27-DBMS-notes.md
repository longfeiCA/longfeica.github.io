---
layout: post
title: Course Notes on RDBMS
subtitle: Database Concepts, Design & Querying Tutorial provided by freeCodeCamp.org
tags: [SQL, database, DBMS]
comments: false
mathjax: false
author: Longfei Jiao
last-updated: 2025-06-02
---



# Course Outline

## Unit 1: Introduction 

    An overview of database management system, Database System Vs File System, Database system concepts, data models. Advantages of DBMS, Schema and instances, Three schema architecture, data independence. Data base languages and interfaces, Disadvantages of DBMS.
    
Data modeling using the Entity Relationship Model:

    ER model concepts, notation for ER diagram, mapping constraints, keys, Concepts of Super Key, candidate key, primary key, Generalization, specialization and aggregation, reduction of an ER diagrams to tables, extended ER model, relationships of higher degree.

### Core Definition

RDBMS:

    Data organized into tables (relations) with rows (tuples) and columns (attributes), governed by domains.

Key concepts: 
 * Relations
 * Attributes
 * Tuples
 * Domains

Benefits & Examples: 

    Ensures data integrity, consistency, and easy accessibility. Popular RDBMS include MySQL (62% market), PostgreSQL (14%), and Oracle (12%).


## Unit 2: Relational Data Model and Relational Algebra

    Relational data model concepts, integrity constraints: entity integrity, referential integrity, Key constraints, Domain constraints. Relational algebra, Operations of relational algebra, queries in relational algebra.

### ER (Entity-Relationship) Modeling

Model Components:

 * Entities represent real-world objects
 * Attributes describe properties
 * Relationships link entities

Advanced Concepts:
 * Cardinality: One-to-one, one-to-many, many-to-many
 * Specialization & Generalization
 * Aggregation for complex relationships

Practical Example
 * University database modeling (Students, courses, and instructors)
 * Using ER diagrams to represent associations and constraints

Fundamental Operations

    Selection, projection, union, intersection, and difference form the query base. 

Joins and Division

    Include theta join, natural join, outer join, and division for complex queries. 

Expressive Power

    Enables formulation of precise queries and manipulation of relational data. 

## Unit 3: Introduction to SQL

    Characteristics of SQL, Advantages of SQL, SQL data types and literals, Types of SQL commands, SQL operators and their procedure, Tables, views and indexes, Insert, update and delete operations, Queries and sub queries, Scalar and Aggregate functions, Joins, Unions, Intersection, Minus.

Core Commands

    SELECT, FROM, WHERE, GROUP BY, HAVING, ORDER BY

DDL & DML

    CREATE, ALTER, DROP, INSERT, UPDATE, DELETE

Advanced Features

 * Joins, Subqueries, Views
 * Indexes, Transactions
 * Security: 65% of breaches involve SQL injection

## Unit 4: Schema Refinement and Normalization

    Functional dependencies, normal forms, first, second, third normal forms, BCNF, inclusion dependencies, loss less join decompositions, normalization using FD, MVD, and JDs.

Functional Dependencies

    Understanding trivial, non-trivial, and full dependencies essential for schema design.

Normal Forms

    Progressing through 1NF, 2NF, 3NF to BCNF to ensure data integrity.

Practical Normalization

    Example: Converting a schema from 1NF to BCNF to eliminate anomalies.

Decomposition Benefits

    Lossless-join and dependency-preserving ensure minimal redundancy and effective data structure.

## Unit 5: Transaction Management and Concurrency Control

    Transaction system, Testing of serializability, Serializability of schedules, conflict and view serializable schedule, recoverability, Recovery form transaction failures, deadlock handling. Concurrency Control Techniques: Concurrency control, locking Techniques for concurrency control.

ACID Principles

    Atomicity, Consistency, Isolation, Durability ensure reliable transactions.

Concurrency Control

    Techniques like locking, timestamping, and optimism prevent conflicts.

Isolation Levels

 * Read uncommitted
 * Read committed
 * Repeatable read
 * Serializable

Recovery & Deadlocks

    Logging, checkpointing for recovery; detection and prevention of deadlocks.

# Full Notes

## Unit 1: Introduction 

### Data

Data consists of raw unorganized facts and details. This includes texts， figures, observations, symbols, and description. It is meaningless until processed. Data may provide information after being processed. 

### Database

Database is a electronic storage where data is stored in a way that it can be easily accessed, managed, and updated. 

### DBMS

#### Definition

Database Management System (DBMS) is a collection of interrelated data and a set of programs to access those data. 

#### Purpose

* Data Integrity: DBMS ensures data consistency and accuracy. 
* Data Security: DBMS provides security features to protect data from unauthorized access, ensuring confidentiality and integrity. 
* Data Access: DBMS allows easy access and retrieval of data. 

### File System

Concurrency and security is limited in FS compared to DBMS. DBMS also offers better structured data management, transactions, and scalability. 

### How to choose a database?

If you need a strictly tabular structured data, RDBMS is the best choice. However, if you need better performance, scalability, or special cases, then columnar, key-value, graph, or document-oriented databases can be good alternatives. 



















# Acknowledgements

[This DBMS course](https://www.youtube.com/watch?v=NdeeSEknp58) is provided by [freeCodeCamp](https://www.youtube.com/@freecodecamp) on YouTube. Essential topics such as ER modeling, relational algebra, SQL, normalization, and transaction management, progressing from foundational principles to advanced applications are discussed in this course. 

# Course Resources

[Youtube Video](https://www.youtube.com/watch?v=NdeeSEknp58)

[Complete Resource Pack & Outline](https://rdbms-resource-pack-650qinf.gamma.site/)

[Details of the course - Unit 1-5](https://pravin-hub-rgb.github.io/BCA/resources/sem3/dbmstbc302/index.html)
