# Lesson 1: The Relational Model

## Learning Goals

- Understand what a Relational Database Management System (RDBMS) is.
- Differentiate between RDBMS and NoSQL databases.
- Identify the key benefits and limitations of PostgreSQL.

## 1. What is an RDBMS?

A **Relational Database Management System (RDBMS)** is a software that stores and retrieves data based on the **relational model**, introduced by E.F. Codd in 1970.

- **Structure**: Data is organized into **tables** (relations) consisting of **rows** (tuples) and **columns** (attributes).
- **Relationships**: Tables can be linked to each other using keys (Primary Keys and Foreign Keys).
- **SQL**: RDBMSs use **Structured Query Language (SQL)** as the standard interface for defining and manipulating data.

PostgreSQL is an **Object-Relational Database (ORDBMS)**, meaning it supports standard relational features but also includes advanced object-oriented features like inheritance and complex custom types.

## 2. PostgreSQL vs. NoSQL

Modern application development often involves choosing between SQL (Relational) and NoSQL (Non-Relational) databases.

| Feature           | PostgreSQL (RDBMS)                                                                               | NoSQL (e.g., MongoDB, Redis, Cassandra)                                                |
| :---------------- | :----------------------------------------------------------------------------------------------- | :------------------------------------------------------------------------------------- |
| **Schema**        | **Rigid**: Structure must be defined before inserting data (Schema-on-write).                    | **Flexible**: Structure can vary per document (Schema-on-read).                        |
| **Relationships** | **Strong**: Built for complex joins and constraints between entities.                            | **Weak**: Often specifically designed to avoid joins (denormalization).                |
| **Consistency**   | **Strong (ACID)**: Data is guaranteed to be valid and committed safely.                          | **Eventual**: Prioritizes availability/speed over immediate consistency (CAP theorem). |
| **Scaling**       | **Vertical**: Typically scaled by adding more power to one server (though sharding is possible). | **Horizontal**: Designed to scale across many cheaper servers.                         |
| **Best Use**      | Financial systems, ERPs, complex business logic, data integrity critical apps.                   | Real-time analytics, content caching, simple document stores, massive write loads.     |

**Why PostgreSQL?**
PostgreSQL bridges the gap. It is a relational database that has excellent support for JSON documents (`JSONB`), allowing you to enjoy the data integrity of SQL with the flexibility of NoSQL in a single system.

## 3. Benefits & Limitations of PostgreSQL

### Benefits

- **Open Source**: Truly free and open source with a permissive license, not owned by a single corporation (unlike MySQL/Oracle).
- **Extensibility**: You can write extensions in C, Python, Perl, etc., and define custom data types and functions.
- **Standards Compliance**: Highly compliant with the SQL standard.
- **Advanced Features**: GIS (PostGIS), JSONB, Full-text Search, Window Functions, Common Table Expressions (CTEs).
- **Reliability**: Known for rock-solid stability and data integrity.

### Limitations

- **Mvcc Bloat**: Its concurrency model (MVCC) creates "dead rows" that must be cleaned up (Vacuuming), which can be complex to tune at scale.
- **Connection Overhead**: PostgreSQL relies on a process-per-connection model, which uses more memory than threading. Connection pooling (e.g., PgBouncer) is almost mandatory for high-scale apps.
- **Write Performance**: While very fast, extreme write-heavy workloads might favor heavily optimized Log-Structured Merge (LSM) tree engines (like Cassandra), though PostgreSQL is catching up.

## Key Takeaways

- PostgreSQL is an **Object-Relational** database that prioritizes data integrity and standards compliance.
- It offers a strict schema structure but provides NoSQL capabilities via `JSONB`.
- It is ideal for complex applications requiring strong consistency and complex querying.
