# Lesson 4: Concurrency & MVCC

## Learning Goals

- Understand the challenge of concurrent access.
- Explain Multi-Version Concurrency Control (MVCC).
- Understand the concept of "Bloat" and why Vacuum is needed.

## 1. The Concurrency Problem

Imagine two users accessing the same bank account row:

- User A reads balance: $100.
- User B reads balance: $100.
- User A withdraws $10 and writes $90.
- User B deposits $20 and writes $120.

If not handled correctly, User A's withdrawal is overwritten/lost by User B's write.
Also, if User A is running a report on all accounts, they shouldn't see money disappear from one account and appear in another halfway through the report.

## 2. What is MVCC?

PostgreSQL solves this using **Multi-Version Concurrency Control (MVCC)**.

Instead of locking rows and making everyone wait (like some older databases), PostgreSQL treats data as **immutable** versions.

- **Updates are Inserts**: When you `UPDATE` a row, Postgres doesn't overwrite the old data. It marks the old row as "dead" (invisible to future transactions) and inserts a _new version_ of the row.
- **Snapshots**: Each transaction sees a "snapshot" of the database at a specific point in time.
  - Readers don't block Writers.
  - Writers don't block Readers.

**Example**:
If a long-running report starts at 1:00 PM, it will see the database exactly as it was at 1:00 PM, even if other users are deleting and adding rows at 1:05 PM. The report query sees the "old versions" of those rows.

## 3. The Cost of MVCC: Bloat

Because updates create new row versions and deletions just mark rows as "dead", the table grows over time with obsolete data. This is called **Bloat**.

These dead rows take up disk space and slow down queries (scanning more data than necessary).

### The Solution: VACUUM

PostgreSQL has a maintenance process called **VACUUM**.

- It scans tables for "dead tuples" (row versions that are no longer visible to any running transaction).
- It marks that space as "free" so it can be reused by future inserts/updates.

**Autovacuum**:
Modern PostgreSQL has an **Autovacuum** daemon that runs in the background, automatically cleaning up bloat. It is critical strictly _never_ to disable Autovacuum.

## Key Takeaways

- **MVCC** allows high concurrency: Readers and Writers don't block each other.
- **Updates create copies**: The old version is kept for older transactions; the new version is for new transactions.
- **Dead Tuples**: Old versions that no one needs anymore.
- **Vacuum**: The process that reclaims space from dead tuples.
