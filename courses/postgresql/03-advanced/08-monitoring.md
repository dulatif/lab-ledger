# Lesson 8: Monitoring & Observability

## Learning Goals

- Enable and use `pg_stat_statements`.
- Monitor active sessions with `pg_stat_activity`.
- Identify key metrics to alert on.

## 1. pg_stat_statements (The Gold Mine)

This is the single most useful extension for performance. It records every query run against the server and aggregates stats (calls, total time, rows).

**Enable it** (`postgresql.conf`):
`shared_preload_libraries = 'pg_stat_statements'`

**Query it**:

```sql
SELECT query, calls, total_exec_time, mean_exec_time
FROM pg_stat_statements
ORDER BY total_exec_time DESC
LIMIT 5;
```

This tells you exactly which queries are eating up your CPU.

## 2. pg_stat_activity (Real-Time)

Who is connected right now? What are they doing?

```sql
SELECT pid, usename, state, query_start, query
FROM pg_stat_activity
WHERE state != 'idle';
```

- **Locked?**: Look for `wait_event_type = 'Lock'`.
- **Stuck?**: Look for queries running for > 1 hour.
- **Kill it**: `SELECT pg_terminate_backend(pid);`

## 3. Key Metrics to Watch

If you set up Prometheus/Grafana, these are the top 4 signals:

1.  **Connections**: Are you reaching `max_connections`? (Need PgBouncer?)
2.  **Replication Lag**: Is the replica falling behind? (Data loss risk).
3.  **Wraparound**: Is Autovacuum failing to keep up? (Critical DB shutdown risk).
4.  **Disk Usage**: Is WAL archiving filling up the disk?

## Key Takeaways

- **Enable `pg_stat_statements` immediately**. It's the first thing a DBA looks at.
- Use `pg_stat_activity` to debug "Why is the DB slow right now?"
- Monitor **Replication Lag** closely in HA setups.
