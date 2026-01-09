# Lesson 1: Streaming Replication

## Learning Goals

- Understand the Primary-Replica architecture.
- Configure Streaming Replication (WAL Streaming).
- Differentiate between Synchronous and Asynchronous replication.

## 1. Physical Replication Basics

Streaming replication sends the WAL records (byte-for-byte changes) from the **Primary** server to one or more **Replica** (Standby) servers.
The Replicas constantly "replay" these WAL records, staying an exact copy of the Primary.

- **Read-Only**: Replicas are strictly Read-Only. You cannot write to them.
- **Primary**: All writes must go to the Primary.

## 2. Configuration Steps

**On Primary**:

1.  Create a replication user: `CREATE ROLE replicator WITH REPLICATION LOGIN...`
2.  `pg_hba.conf`: Allow the replica to connect.
3.  `postgresql.conf`:
    - `wal_level = replica`
    - `max_wal_senders = 10`

**On Replica**:

1.  Start with an empty data directory.
2.  Run `pg_basebackup` with the `-R` flag (Write configuration):
    ```bash
    pg_basebackup -h primary_ip -U replicator -D /var/lib/postgresql/data -R -P
    ```
3.  Start the replica server. It will automatically connect to the primary and start streaming.

## 3. Sync vs Async

**Asynchronous (Default)**:

- Primary writes to disk -> Commits to Client -> Sends to Replica later.
- _Pros_: Fast. Replica latency doesn't slow down the Primary.
- _Cons_: Data Loss Risk. If Primary dies before sending WAL, that data is lost.

**Synchronous**:

- Primary writes to disk -> Sends to Replica -> Waits for Replica acknowledgement -> Commits to Client.
- _Pros_: Zero Data Loss (RPO = 0).
- _Cons_: Slower writes. If Replica dies, Primary freezes (cannot commit).

## Key Takeaways

- **Streaming Replication** = High Availability + Read Scaling.
- **Replica** is an exact binary copy of the Primary.
- Use **Async** for performance (most common); use **Sync** for financial correctness.
