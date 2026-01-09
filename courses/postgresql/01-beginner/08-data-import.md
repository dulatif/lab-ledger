# Lesson 8: Data Import/Export (COPY)

## Learning Goals

- Understand the difference between `INSERT` and `COPY`.
- Import a CSV file into a table.
- Export query results to a CSV file.

## 1. Why `COPY`?

If you have a CSV file with 1 million rows:

1.  **Running 1 million INSERTs**: Extremely slow. Every row has parsing overhead, network latency, and transaction log overhead.
2.  **Using `COPY`**: Extremely fast. It streams data directly into the table structure, often optimizing the WAL writing process.

**Rule of Thumb**: For bulk data loading, always use `COPY`.

## 2. Importing Data (COPY FROM)

Scenario: You have `users.csv` on the server.

```csv
Alice,alice@example.com
Bob,bob@example.com
```

**Server-Side Import (Superuser only)**:
This reads a file _on the database server's filesystem_.

```sql
COPY app_data.users (full_name, email)
FROM '/path/to/users.csv'
WITH (FORMAT csv, HEADER false);
```

**Client-Side Import (`\copy`)**:
Usually, the file is on _your_ laptop, not the server. `psql` provides the `\copy` meta-command to stream your local file to the server.

```bash
# Run this inside psql
\copy app_data.users (full_name, email) FROM 'C:/users.csv' WITH (FORMAT csv, HEADER false);
```

## 3. Exporting Data (COPY TO)

You can dump a whole table or just the results of a specific query.

**Export Table**:

```sql
\copy app_data.users TO 'users_backup.csv' WITH (FORMAT csv, HEADER true);
```

**Export Query Results**:
Great for generating reports.

```sql
\copy (SELECT * FROM app_data.users WHERE is_active = true) TO 'active_users.csv' WITH (FORMAT csv, HEADER true);
```

## 4. Handling Errors

If the import fails halfway (e.g., duplicate email violation at row 500,000), the _entire_ transaction rolls back. Nothing is imported.

To save the valid rows and ignore the bad ones, or log errors separately, newer Postgres versions (17+) include `ON_ERROR` options, but formerly you had to script this outside the DB.

## Key Takeaways

- Use `COPY` for bulk operations; it is orders of magnitude faster than `INSERT`.
- `COPY` is SQL (server-side); `\copy` is a `psql` command (client-side).
- You can export complex query results directly to CSV.
