# Lesson 6: Backup & Restore

## Learning Goals

- `mongodump`.
- Point-in-Time Recovery.

## 1. Logical Backups

`mongodump` / `mongorestore`.
Exports data to BSON files.
Good for small datasets or Development. Slow for TBs.

## 2. Physical Backups (Snapshots)

Copying the underlying data files (`/data/db`).
Fastest method. used by Ops Manager / Atlas Backup.
Requires filesystem support (LVM snapshots, EBS snapshots).

## 3. Continuous Backups

Using the Oplog to replay events. Allows restoring to a specific timestamp (Point-in-Time Recovery).

## Key Takeaways

- Test your backups! A backup is useless if you can't restore it.
- Atlas Automates this entirely.
