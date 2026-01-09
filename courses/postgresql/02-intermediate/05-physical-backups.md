# Lesson 5: Physical Backups & PITR

## Learning Goals

- Understand Physical Backups (Binary Copy).
- Understand Point-In-Time Recovery (PITR).
- Use `pg_basebackup`.

## 1. What is a Physical Backup?

A physical backup copies the actual data files (the `base` directory) from disk.

- **Pros**: Extremely fast restore (just copy files back), supports Point-In-Time Recovery.
- **Cons**: Not portable (must be same OS, same Postgres major version), backs up the entire cluster (cannot restore just one table).

## 2. Point-In-Time Recovery (PITR)

Imagine someone accidentally drops a critical table at **2:00 PM**.
With a Logical Backup from last night (12:00 AM), you lose 14 hours of data.

With **PITR**, you can restore the database to the state it was in at **1:59 PM**, losing only 1 minute of data.
**How?**

1.  **Base Backup**: You have a physical copy of the files from 12:00 AM.
2.  **WAL Archive**: You have a saved history of every transaction (WAL files) that happened since 12:00 AM.
3.  **Replay**: You assume the 12:00 AM state and "replay" the WAL logs until you hit the timestamp 1:59 PM.

## 3. Using `pg_basebackup`

This is the built-in tool for streaming a physical copy of the database.

```bash
# -D (Directory), -Fp (Plain format), -Xs (Stream WAL during backup)
pg_basebackup -h localhost -U postgres -D /backup/daily_backup -Fp -Xs -P
```

## 4. Setting up WAL Archiving

To enable PITR, you must tell Postgres to send completed WAL files to a safe location (e.g., an S3 bucket or a separate NAS).

`postgresql.conf`:

```conf
wal_level = replica
archive_mode = on
archive_command = 'cp %p /mnt/backup_server/wal_archive/%f'
# In production, use a tool like pgBackRest instead of 'cp'
```

## Key Takeaways

- **Physical Backups** are required for large production databases (> 100GB) where logical restore time is too slow.
- **PITR** is the gold standard for disaster recovery. It minimizes RPO (Recovery Point Objective - how much data you lose).
- **pg_basebackup** creates the foundation; **WAL Archiving** provides the continuous history.
