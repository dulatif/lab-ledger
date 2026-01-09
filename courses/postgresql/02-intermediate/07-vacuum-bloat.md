# Lesson 7: Vacuum & Bloat

## Learning Goals

- Deeply understand "Dead Tuples" and Bloat.
- Configure Autovacuum.
- Differentiate `VACUUM` vs `VACUUM FULL`.

## 1. The Bloat Problem

As discussed in the MVCC lesson, updates and deletes leave "dead tuples" behind.
If these aren't cleaned up, the table file grows physically larger and larger, filled with junk.

- **Bloat**: The percentage of disk space occupied by dead tuples.
- **Impact**: Queries must scan more disk pages to find the live data -> Slower Performance.

## 2. VACUUM (Standard)

The `VACUUM` command scans the table and marks the space occupied by dead tuples as "available for reuse."

- **Key Behavior**: It does **NOT** return disk space to the Operating System. It just makes holes in the cheese for new data to fill.
- **Locking**: Non-blocking. Production traffic can continue normally.

```sql
VACUUM VERBOSE ANALYZE app_data.users;
```

- `ANALYZE`: Also updates the table statistics (row counts) for the query planner. Always good to include.

## 3. VACUUM FULL (The Nuclear Option)

If a table is 90% bloat, simply marking space for reuse isn't enough. You want to shrink the file.

`VACUUM FULL` rewrites the _entire table_ into a new file, physically removing the dead space.

- **Locking**: **EXCLUSIVE LOCK**. No one can read or write to the table while it runs.
- **Impact**: In production, this causes downtime for that table. Avoid unless absolutely necessary.

## 4. Tuning Autovacuum

PostgreSQL has a daemon that runs `VACUUM` automatically.
Common Configs in `postgresql.conf`:

- `autovacuum = on` (Must be on)
- `autovacuum_vacuum_scale_factor = 0.1` (Default 0.2): Run vacuum when 10% of the table has changed, instead of 20%. Good for large tables.
- `autovacuum_vacuum_cost_limit`: Increase this to allow vacuum to work faster/harder on modern SSDs.

## Key Takeaways

- **Dead Tuples** cause bloat.
- **Standard VACUUM** reclaims space for internal reuse (non-blocking).
- **VACUUM FULL** reclaims disk space but locks the table (blocking).
- **Autovacuum** is your friend. Tune it to be more aggressive on busy tables.
