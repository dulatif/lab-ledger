# Lesson 4: Logical Backups

## Learning Goals

- Understand what a logical backup is.
- Use `pg_dump` and `pg_restore`.
- Perform full cluster backups with `pg_dumpall`.

## 1. What is a Logical Backup?

A logical backup extracts the _SQL commands_ needed to recreate the database (e.g., millions of `INSERT` statements or `COPY` commands).

- **Pros**: Portable across architectures (move data from Linux to Windows, or x86 to ARM), allows partial restores (single table), upgrades across Postgres major versions.
- **Cons**: Slower to restore (requires re-indexing and executing SQL), CPU intensive during backup.

## 2. Using `pg_dump`

`pg_dump` backs up a **single database**. It produces a consistent snapshot even if users are writing during the backup.

**Plain Text Format (Default)**:
Creates a readable `.sql` script.

```bash
pg_dump -U postgres mydatabase > backup.sql
```

_Restore_: `psql -U postgres -d new_db -f backup.sql`

**Custom Format (Recommended)**:
Creates a binary file. It is compressed and allows parallel restore.

```bash
# -F c (Custom format), -f (filename)
pg_dump -U postgres -F c -f backup.dump mydatabase
```

## 3. Using `pg_restore`

The custom format relies on `pg_restore`.

```bash
# -j 4 (Use 4 CPU cores for parallel restore - MUCH faster)
# -d new_db (Target database)
pg_restore -U postgres -j 4 -d new_db backup.dump
```

**Selective Restore**:
You can pull just one table from the dump:

```bash
pg_restore -U postgres -d new_db -t specific_table backup.dump
```

## 4. `pg_dumpall`

`pg_dump` only backs up one database content. It does **NOT** backup global objects like **Users (Roles)** or **Groups**.

`pg_dumpall` extracts the entire cluster (all databases + roles).

```bash
pg_dumpall -U postgres --globals-only > globals.sql
```

_Tip_: It is good practice to run a daily `globals-only` backup alongside your individual DB backups.

## Key Takeaways

- **Always prefer Custom Format (`-F c`)** for `pg_dump`. It saves space and enables parallel restores.
- **Logical backups are essential for upgrades**: You can't just copy binary files to a new major version of Postgres.
- **Don't forget Globals**: Use `pg_dumpall --globals-only` to save your users and passwords.
