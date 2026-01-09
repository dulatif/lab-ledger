# Lesson 6: Service Management & Connectivity

## Learning Goals

- Start, stop, and restart the PostgreSQL service.
- Connect to the database using `psql`.
- Troubleshoot basic connection issues.

## 1. Managing the Service

How you control the server depends on your OS.

### Linux (Systemd)

```bash
sudo systemctl start postgresql
sudo systemctl stop postgresql
sudo systemctl restart postgresql
sudo systemctl status postgresql
```

### macOS (Homebrew)

```bash
brew services start postgresql
brew services stop postgresql
```

### Universal Method (`pg_ctl`)

If you don't have root access or are using a custom data directory, use the native `pg_ctl` tool.

```bash
# Start
pg_ctl -D /path/to/data -l logfile start

# Stop
pg_ctl -D /path/to/data stop

# Reload (Applies config changes without killing connections)
pg_ctl -D /path/to/data reload
```

## 2. Connecting with `psql`

`psql` is the standard terminal client.

**Basic Connection**:

```bash
# psql -h [host] -p [port] -U [user] -d [database]
psql -h localhost -U postgres
```

- If you omit `-h`, it tries to connect via a "Unix Socket" (file-based connection), which is faster than TCP/IP.
- The default user is `postgres`.

**Useful `psql` Meta-Commands**:
Once inside `psql` (the prompt looks like `postgres=#`), you can use standard SQL _or_ special commands starting with a backslash `\`:

- `\l` : List all databases.
- `\c dbname` : Connect/Switch to a different database.
- `\dt` : List all tables in the current schema.
- `\du` : List all users/roles.
- `\d table_name`: Describe the structure (columns) of a table.
- `\q` : Quit/Exit.

## 3. Connection Strings

Applications (Node.js, Python, etc.) usually connect using a URL format:

```text
postgresql://[user]:[password]@[host]:[port]/[database]
```

Example:
`postgresql://myuser:secret@localhost:5432/myapp_db`

## 4. Troubleshooting

- **"Connection refused"**: Is the server running? (`systemctl status`). Is it listening on that port?
- **"Peer authentication failed"**: By default on Linux, the `postgres` system user is expected to match the `postgres` OS user ("Peer" auth).
  - _Fix_: Run `sudo -u postgres psql`.
- **"FATAL: role 'xxx' does not exist"**: You are trying to log in as a user that hasn't been created in the database yet.

## Key Takeaways

- `pg_ctl reload` is safer than `restart` because it doesn't disconnect active users.
- Master the `\d` commands in `psql`; they are faster than finding the documentation.
- Use Connection Strings for your application configs.
