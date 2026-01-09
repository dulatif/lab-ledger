# Lesson 3: Architecture 101

## Learning Goals

- Define ACID compliance.
- Understand the concept of a Transaction.
- Explain the role of the Write-Ahead Log (WAL) in durability.

## 1. ACID Compliance

PostgreSQL is fully **ACID** compliant. This is the promise the database makes to you about data reliability.

- **A - Atomicity**: "All or Nothing". If a series of steps constitutes a transaction, either all of them happen, or none of them do. If an error occurs halfway, the database rolls back to the state before the transaction started.
- **C - Consistency**: The database must go from one valid state to another valid state. It enforces rules (constraints, foreign keys) continuously.
- **I - Isolation**: Transactions occurring at the same time should not interfere with each other. If I am reading a balance while you are updating it, I shouldn't see a "half-updated" state.
- **D - Durability**: Once a transaction is committed, it stays committed, even if the power plug is pulled immediately after.

## 2. Transactions

A transaction is a unit of work. In SQL, you control this explicitly:

```sql
BEGIN; -- Start the transaction

INSERT INTO accounts (user_id, balance) VALUES (1, 1000);
UPDATE inventory SET stock = stock - 1 WHERE item_id = 50;

COMMIT; -- Save changes permanently
-- OR
ROLLBACK; -- Undo everything since BEGIN
```

If you don't type `BEGIN`, PostgreSQL wraps every single command in its own implicit transaction (Auto-commit).

## 3. The Write-Ahead Log (WAL)

How does PostgreSQL guarantee **Durability** (the 'D' in ACID) without being incredibly slow?

Writing solely to the actual data files (called "heap" files) is slow because it requires random I/O (jumping around the disk) to find the right rows.

**The Solution: WAL**

1.  **Append-Only**: When you modify data, Postgres first writes the change to the **Write-Ahead Log**. This is an append-only file, which implies **Sequential I/O** (very fast).
2.  **Commit**: Once the entry hits the WAL on disk, the transaction is considered "Committed" and safe.
3.  **Checkpoint**: Later, in the background, a "Checkpointer" process takes these changes and updates the main data files (Heap) efficiently.

**Crash Recovery**:
If the server crashes (power loss), the main data files might be outdated or corrupt. On restart, Postgres reads the WAL from the last checkpoint and "replays" all the transactions to bring the database back to a consistent state.

## Key Takeaways

- **ACID** ensures your data is safe and logical.
- **Transactions** group multiple steps into a single atomic operation.
- **WAL** is the mechanism for Durability. Data is written to the log _before_ being written to the final table storage.
