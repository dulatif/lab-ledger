# PostgreSQL Beginner Course Syllabus

## Module 1: Database Fundamentals

**Goal**: Understand the internal architecture of PostgreSQL and core relational concepts.

1.  **The Relational Model**
    - Concepts: Relational Databases vs. NoSQL, RDBMS Benefits/Limitations.
    - Goal: Understand when and why to use PostgreSQL.
2.  **Core Concepts**
    - Concepts: Tables, Schemas, Rows, Columns, Data Types.
    - Goal: Master the basic building blocks of data storage.
3.  **Architecture 101**
    - Concepts: ACID compliance, Transactions, Write-ahead Log (WAL).
    - Goal: Grasp how PostgreSQL ensures data integrity.
4.  **Concurrency Basics**
    - Concepts: MVCC (Multi-Version Concurrency Control).
    - Goal: Understand how PostgreSQL handles simultaneous access.

## Module 2: Installation & Basic Operations

**Goal**: Install the server, manage the service, and execute basic operations.

5.  **Installation & Setup**
    - Concepts: Package Managers, Docker, Source Build, Initial Config.
    - Goal: Get a running PostgreSQL instance.
6.  **Service Management & Connectivity**
    - Concepts: `systemd`, `pg_ctl`, `psql`, Connection strings.
    - Goal: Start/Stop server and connect via CLI.
7.  **Basic SQL Operations**
    - Concepts: DDL (CREATE SCHEMA/TABLE), DML (INSERT/UPDATE/DELETE).
    - Goal: Create structures and modify data.
8.  **Data Import/Export**
    - Concepts: `COPY` command.
    - Goal: Efficiently bulk load and unload data.
