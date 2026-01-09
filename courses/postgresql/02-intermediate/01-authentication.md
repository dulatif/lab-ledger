# Lesson 1: Authentication & pg_hba.conf

## Learning Goals

- Understand strict vs. permissive access controls.
- Configure `pg_hba.conf` to securely control client connections.
- Differentiate between authentication methods (Peer, MD5, Scram-SHA-256).

## 1. What is pg_hba.conf?

PostgreSQL uses a file called `pg_hba.conf` (Host-Based Authentication) to decide who can connect to the server.

- **Location**: Usually in the data directory (e.g., `/var/lib/pgsql/data/pg_hba.conf`).
- **Process**: When a connection attempt arrives, Postgres scans this file from **top to bottom**. It applies the rule from the _first matching line_.

**Format**:

```text
# TYPE  DATABASE        USER            ADDRESS                 METHOD
local   all             postgres                                peer
host    all             all             127.0.0.1/32            scram-sha-256
host    all             all             0.0.0.0/0               reject
```

## 2. Authentication Methods (The "METHOD" column)

- **trusted**: Trust everyone. Dangerous. Never use this on a network socket.
- **reject**: Reject the connection immediately. Good for blacklisting IPs at the top of the file.
- **peer**: (Linux only) Expects the OS user name to match the DB user name. This is why you must type `sudo -u postgres psql`.
- **md5**: Legacy password hashing. Vulnerable to certain attacks. Avoid if possible.
- **scram-sha-256**: The modern, secure standard for password authentication. **Use this.**
- **cert**: Requires the client to present a valid SSL certificate signed by a trusted CA.

## 3. Configuration Setup

**Scenario**: You want to allow your web application (IP `10.0.0.5`) to connect as user `app_user` with a password, but block everyone else from the internet.

Edit `pg_hba.conf`:

```conf
# TYPE  DATABASE    USER        ADDRESS         METHOD

# 1. Admin local access (Peer is safe for local root)
local   all         postgres                    peer

# 2. Allow Application Server
host    my_app_db   app_user    10.0.0.5/32     scram-sha-256

# 3. Allow Developer VPN Subnet
host    all         all         192.168.1.0/24  scram-sha-256

# 4. Default: Explicitly reject everything else (Good practice, though implicit default is also reject)
host    all         all         0.0.0.0/0       reject
```

**Applying Changes**:
You must reload the server for changes to take effect:

```bash
pg_ctl reload
# OR inside SQL
SELECT pg_reload_conf();
```

## 4. Listen Addresses

Even if `pg_hba.conf` allows connections, the server must be listening on the network interface.
Check `postgresql.conf`:

- `listen_addresses = 'localhost'` (Default: only local connections).
- `listen_addresses = '*'` (Listen on all interfaces/public internet).

## Key Takeaways

- **First Match Wins**: Order matters in `pg_hba.conf`.
- **Use scram-sha-256**: It prevents password sniffing and replay attacks better than md5.
- **Reload**: Don't forget to reload configuration after edits.
