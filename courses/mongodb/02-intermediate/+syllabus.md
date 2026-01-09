# MongoDB Intermediate Syllabus

## Module 4: Aggregation Framework

**Goal**: Advanced data analysis.

1.  **Introduction to Pipelines**
    - Concepts: Stages, `$match`, `$group`, `$sort`.
    - Goal: Like UNIX pipes or SQL Group By.
2.  **Data Transformation**
    - Concepts: `$project`, `$addFields`, `$unwind`.
    - Goal: Reshaping documents.
3.  **Lookup (Joins)**
    - Concepts: `$lookup`.
    - Goal: Joining collections (Left Outer Join).

## Module 5: Indexing & Performance

**Goal**: Speed.

4.  **Single & Compound Indexes**
    - Concepts: B-Trees, ESR Rule (Equality, Sort, Range).
    - Goal: Avoiding Collection Scans.
5.  **Query Analysis**
    - Concepts: `explain()`, Winning Plan, Index Covered Queries.
    - Goal: Debugging slow queries.
6.  **Special Indexes**
    - Concepts: Multikey (Arrays), Text, TTL.
    - Goal: Specific use cases.

## Module 6: Transactions & Validation

**Goal**: Integrity.

7.  **Schema Validation**
    - Concepts: JSON Schema validation rules.
    - Goal: Enforcing structure in the DB.
8.  **ACID Transactions**
    - Concepts: `session`, `startTransaction`, `commitTransaction`.
    - Goal: Multi-document atomicity.
