# Lesson 8: Configuration Tuning

## Learning Goals

- Navigate `postgresql.conf`.
- Tune memory parameters (`shared_buffers`, `work_mem`).
- Understand Checkpoints and WAL tuning.

## 1. postgresql.conf

The main configuration file.

- **Location**: `SHOW config_file;` in SQL will tell you where it is.
- **Reload vs Restart**:
  - Most settings (like `work_mem`) can be applied with a **Reload** (`pg_ctl reload`).
  - Core settings (like `shared_buffers`) require a **Restart**.

## 2. Memory Parameters

Postgres doesn't manage memory like Java (Heap). It relies heavily on the OS filesystem cache.

### `shared_buffers`

- **What**: How much memory Postgres dedicates to caching data pages.
- **Rule of Thumb**: Set to **25% of System RAM**.
  - Example: On a 16GB server, set to 4GB.
  - Why not 100%? Because Postgres relies on the standard Linux Filesystem Cache for the rest.

### `work_mem`

- **What**: Memory allowing for _each_ sort/hash operation (ORDER BY, DISTINCT, JOIN).
- **Scope**: Per operation, per user. **Dangerous**. If you set this to 100MB and have 100 users running complex queries, you use 10GB of RAM instantly.
- **Rule of Thumb**: Start low (4MB - 16MB). Increase specifically for reporting users.

### `maintenance_work_mem`

- **What**: Memory for maintenance tasks (VACUUM, CREATE INDEX).
- **Rule of Thumb**: Higher is better (e.g., 1GB). Only a few run at a time.

## 3. Storage & WAL Tuning

Everything you write goes to the WAL first.

- `wal_buffers`: Memory for unwritten WAL data. (Default 16MB is usually fine, maybe bump to 64MB).
- **Checkpoints**:
  - `min_wal_size` / `max_wal_size`: Increase `max_wal_size` (e.g., 4GB or higher) to reduce the frequency of checkpoints. Frequent checkpoints hurt write performance.

## 4. PGTune

Don't guess. Use the community standard tool: **PGTune** (https://pgtune.leopard.in.ua/).
Input your hardware stats, and it gives you a solid baseline config.

## Key Takeaways

- **25% RAM for `shared_buffers`**.
- **Be careful with `work_mem`**; it can cause Out-Of-Memory (OOM) crashes if too high.
- Use **PGTune** to generate your initial configuration.
