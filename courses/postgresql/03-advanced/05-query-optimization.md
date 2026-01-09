# Lesson 5: Query Optimization & EXPLAIN

## Learning Goals

- Read and understand `EXPLAIN ANALYZE` output.
- Identify Sequential Scans vs Index Scans.
- Understand Cost metrics.

## 1. Using EXPLAIN

Prefix any query with `EXPLAIN` to see the plan without running it.
Prefix with `EXPLAIN ANALYZE` to **run it** and measure actual timing.

```sql
EXPLAIN ANALYZE SELECT * FROM users WHERE email = 'bob@example.com';
```

**Output Breakdown**:

```text
Index Scan using users_email_key on users  (cost=0.28..8.29 rows=1 width=500) (actual time=0.045..0.046 rows=1 loops=1)
  Index Cond: (email = 'bob@example.com'::text)
Planning Time: 0.120 ms
Execution Time: 0.080 ms
```

## 2. Key Scan Types

1.  **Seq Scan (Sequential Scan)**: Reading every single page of the table.
    - _Bad_: If looking for 1 needle in a haystack (1B rows).
    - _Good_: If reading 90% of the table (Aggregation).
2.  **Index Scan**: Jumping to the exact location using the B-Tree.
    - _Good_: For high selectivity (WHERE id = 5).
    - _Slow_: Requires "Random I/O" if scanning many rows.
3.  **Bitmap Heap Scan**: Hybrid. Uses index to find locations, creates a bitmap in memory, then reads the table sorted by physical location.

## 3. Understanding Costs

`cost=0.28..8.29`

- **Startup Cost (0.28)**: Cost to fetch the _first_ row.
- **Total Cost (8.29)**: Cost to fetch _all_ rows.
- The units are arbitrary (1.0 = cost of reading one page sequentially).

**Why it matters**: The planner picks the path with the lowest Total Cost. If statistics are wrong (e.g., it thinks table has 5 rows but it has 5M), it will pick the wrong plan (e.g., Nested Loop Join instead of Hash Join).

## 4. Optimization Strategy

1.  **Identify slow query** (`pg_stat_statements`).
2.  **Run EXPLAIN ANALYZE**.
3.  **Check**: Is it doing a Seq Scan on a large table? -> **Add Index**.
4.  **Check**: Is actual rows (100000) vastly different from estimated rows (5)? -> **Run ANALYZE**.

## Key Takeaways

- `EXPLAIN ANALYZE` is the truth.
- **Seq Scans** are the enemy of low-latency lookups.
- **Statistics** drive the planner. Keep them updated.
