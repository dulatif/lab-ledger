# Lesson 6: Enterprise Tools (pgBackRest)

## Learning Goals

- Compare Barman vs. pgBackRest.
- Understand why custom scripts with `cp` are dangerous.
- Overview of `pgBackRest` architecture.

## 1. Why Third-Party Tools?

While `pg_dump` and `pg_basebackup` are built-in, they lack management features:

- Retention Policy: "Delete backups older than 30 days."
- Compression & Encryption: Encrypting backups before sending to S3.
- Parallelism: Maximizing throughput.
- Differential/Incremental Backups: "Only backup what changed since yesterday."

## 2. Comparison: Barman vs pgBackRest

Both are excellent open-source tools.

| Feature         | Barman                   | pgBackRest                              |
| :-------------- | :----------------------- | :-------------------------------------- |
| **Language**    | Python                   | C                                       |
| **Performance** | Good                     | **Excellent** (No interpreter overhead) |
| **S3 Support**  | Yes                      | Native & High Performance               |
| **Incremental** | Yes (using rsync or WAL) | Yes (Block level)                       |
| **Complexity**  | Moderate                 | High (Lots of config options)           |

**Industry Verdict**: `pgBackRest` has largely become the standard for high-performance PostgreSQL environments (especially Kubernetes operators like CrunchyData) due to its speed and native S3 integration.

## 3. pgBackRest Architecture

pgBackRest uses a "Repository" concept.

1.  **Stanza**: A configuration defining a database cluster.
2.  **Repo**: Where files are stored (Local disk, S3, GCS, Azure).

**Basic Workflow**:

- **Full Backup**: Copies the entire database.
- **Incremental Backup**: Scopies only files changed since the last backup.
- **Differential Backup**: Copies only files changed since the last _Full_ backup.

**Commands**:

```bash
# Initialize the repository
pgbackrest --stanza=main stanza-create

# Run a backup
pgbackrest --stanza=main backup

# Check info
pgbackrest info
```

## 4. Validation

A backup is worthless if it cannot be restored.
pgBackRest calculates checksums for every file during backup and verifies them during restore. It also allows you to perform a "Delta Restore" (healing a broken cluster by only downloading corrupted files) which is much faster than a full restore.

## Key Takeaways

- **Don't write shell scripts**: DO NOT use `cp` or `rsync` for archiving WALs in production. Use a tool that verifies checksums (pgBackRest).
- **Incremental Backups**: Save massive amounts of storage and time.
- **pgBackRest**: The recommended tool for modern, large-scale Postgres deployments.
