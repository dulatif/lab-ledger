# Lesson 2: Logical Replication

## Learning Goals

- Understand how Logical Replication differs from Physical.
- Implement `PUBLICATION` and `SUBSCRIPTION`.
- Explore use cases (ETL, Upgrades, Selective Replication).

## 1. Physical vs Logical

| Feature        | Physical (Streaming)     | Logical                                             |
| :------------- | :----------------------- | :-------------------------------------------------- |
| **Unit**       | Entire Cluster (All DBs) | Per Table / Per Database                            |
| **Changes**    | Binary WAL Blocks        | Decoded Rows (Insert/Update/Delete)                 |
| **Version**    | Must be identical        | Can replicate between v14 -> v16                    |
| **Restraints** | Read-Only Replica        | Replica is Writable (can have own indexes/triggers) |

## 2. Terminology

- **Publisher**: The source node that owns the data.
- **Subscriber**: The downstream node receiving changes.

## 3. Configuration

**Step 1: Publisher**
`postgresql.conf`: `wal_level = logical`

```sql
-- Create Publication for specific tables
CREATE PUBLICATION my_pub FOR TABLE users, orders;
```

**Step 2: Subscriber**

```sql
-- Create Subscription connecting to Publisher
CREATE SUBSCRIPTION my_sub
CONNECTION 'host=publisher_ip dbname=mydb user=rep_user password=secret'
PUBLICATION my_pub;
```

## 4. Use Cases

1.  **Zero-Downtime Upgrades**: Replicate from Old Version -> New Version.
2.  **ETL / Data Warehousing**: Replicate only the `sales` table to an analytics DB, while ignoring the `logs` table.
3.  **Geo-Distribution**: Replicate regional data to a central HQ database.

## Key Takeaways

- **Logical Replication** works at the row level, offering flexibility that Streaming Replication lacks.
- It allows the subscriber to have **different indexes** or **triggers** (great for reporting).
- Unlike Streaming, the Subscriber database is **writable** (but don't modify replicated rows or you'll break the sync).
