# Lesson 9: Troubleshooting

## Learning Goals

- Analyze Log Files effectively.
- Use `pgBadger` to visualize logs.
- Debug Lock contention.

## 1. Logging Configuration

Default Postgres logging is very quiet. Turn it up for production.
`postgresql.conf`:

```conf
logging_collector = on
log_min_duration_statement = 1000  # Log any query taking > 1s
log_checkpoints = on
log_lock_waits = on
log_temp_files = 0                 # Log when queries spill to disk
```

## 2. pgBadger

Reading raw text logs is hard. `pgBadger` is a Perl script that parses the log file and generates a beautiful HTML report.

- "Top Slowest Queries"
- "Most Frequent Errors"
- "Histogram of Query Times"

## 3. Lock Contentions

Scenario: An `ALTER TABLE` (Exclusive Lock) is stuck waiting for a `SELECT` (Share Lock) to finish. Meanwhile, all other `SELECT`s build up behind the `ALTER TABLE`. The site goes down.

**Diagnosis**:

```sql
SELECT * FROM pg_locks pl LEFT JOIN pg_stat_activity psa
ON pl.pid = psa.pid;
```

Look for `granted = f` (False).

**Prevention**:

- Never run DDL (`ALTER TABLE`) during peak hours.
- Set `lock_timeout` for migrations (e.g., `SET lock_timeout = '2s'`). If it can't get the lock instantly, it fails rather than queueing up and taking the site down.

## 4. System Level

Sometimes it's not Postgres.

- `top` / `htop`: Is CPU 100%?
- `iostat`: Is Disk I/O 100%? (Maybe a backup is running?)
- `dmesg`: Did the OOM Killer kill Postgres? ("Out of memory: Kill process...")

## Key Takeaways

- **Log Slow Queries**: Set `log_min_duration_statement`.
- **Use lock_timeout**: Prevent migrations from causing outages.
- **Check dmesg**: If Postgres crashed/restarted randomly, it was probably the OOM Killer.
