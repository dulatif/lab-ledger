# PostgreSQL Advanced Course Syllabus

## Module 6: High Availability & Replication

**Goal**: Architect resilient database clusters.

1.  **Streaming Replication**
    - Concepts: Primary/Replica setup, Sync vs Async, Replication Slots.
    - Goal: Set up a basic HA cluster.
2.  **Logical Replication**
    - Concepts: Publication/Subscription, Conflict handling, Partial replication.
    - Goal: Replicate data across versions or selective tables.
3.  **High Availability & Clustering**
    - Concepts: Patroni, Leader Election, PgBouncer (Pooling).
    - Goal: Understand automated failover and connection scaling.

## Module 7: Performance Tuning & Internals

**Goal**: Optimize performance at the lowest levels.

4.  **Internals Deep Dive**
    - Concepts: Process Architecture, Heap files, TOAST, Buffer tagging.
    - Goal: Visualize how PostgreSQL stores and retrieves pages.
5.  **Query Optimization & EXPLAIN**
    - Concepts: Reading Explain Plans, Costs, Scan types (Seq/Index/Bitmap).
    - Goal: Debug and fix slow queries.
6.  **Advanced Indexing**
    - Concepts: GIN, GiST, BRIN, Partial/Functional Indexes.
    - Goal: Optimize for specific data types (JSONB, GIS, Time-series).

## Module 8: Scaling & Troubleshooting

**Goal**: Scale beyond a single node and solve complex production issues.

7.  **Partitioning & Sharding**
    - Concepts: Declarative Partitioning, Trigger-based, Citus (intro).
    - Goal: Manage massive tables.
8.  **Monitoring & Observability**
    - Concepts: `pg_stat_statements`, `pg_stat_activity`, Prometheus/Grafana basics.
    - Goal: Identify bottlenecks in production.
9.  **Advanced Troubleshooting**
    - Concepts: Lock analysis, `pgBadger` log analysis, System-level profiling (`perf` concept).
    - Goal: Root cause analysis for crashes or stalls.
