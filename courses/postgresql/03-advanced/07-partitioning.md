# Lesson 7: Partitioning & Sharding

## Learning Goals

- Implement Declarative Partitioning for massive tables.
- Understand data sharding concepts (Citus).
- Create and manage partitions.

## 1. Why Partition?

If you have a table with 1 Billion rows (e.g., `logs`), queries like `DELETE FROM logs WHERE date < '2022-01-01'` become impossibly slow (VACUUM struggles).

**Partitioning** splits this one logical table into many smaller physical tables (e.g., `logs_2022`, `logs_2023`).

- **Performance**: Queries for "2023" only scan the `logs_2023` table (Partition Pruning).
- **Maintenance**: To delete old data, you simply `DROP TABLE logs_2022`. This is instant and causes zero bloat.

## 2. Declarative Partitioning (Native)

Postgres 10+ made this easy.

**Step 1: Create Main Table**

```sql
CREATE TABLE access_logs (
    id SERIAL,
    log_date DATE NOT NULL,
    message TEXT
) PARTITION BY RANGE (log_date);
```

**Step 2: Create Partitions**
You must manually create the partitions (or use a script/pg_partman).

```sql
CREATE TABLE access_logs_2023 PARTITION OF access_logs
    FOR VALUES FROM ('2023-01-01') TO ('2024-01-01');

CREATE TABLE access_logs_default PARTITION OF access_logs DEFAULT;
```

**Step 3: Query**

```sql
SELECT * FROM access_logs WHERE log_date = '2023-05-01';
```

Postgres automatically routes this to `access_logs_2023`.

## 3. Sharding (Horizontal Scaling)

Partitioning splits a table on _one_ server. Sharding splits a table across _many_ servers.
Postgres does not do this natively. You need an extension like **Citus**.

**Citus**:

- Turns Postgres into a distributed database.
- Tables are "sharded" across multiple worker nodes.
- The "Coordinator" node parallels the query to all workers and aggregates the results.

## Key Takeaways

- **Partitioning** is for managing large tables (TB scale) on a single node.
- **Partition Pruning** speeds up queries by skipping irrelevant files.
- **Sharding (Citus)** is for infinite scaling across many nodes.
