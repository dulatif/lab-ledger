# MongoDB Advanced Syllabus

## Module 7: Replication & Sharding

**Goal**: Scale to infinity.

1.  **Replica Sets (High Availability)**
    - Concepts: Primary, Secondary, Arbiter, Oplog.
    - Goal: Zero downtime.
2.  **Sharding (Horizontal Scaling)**
    - Concepts: Shard Keys, Chunks, Mongos (Router), Config Servers.
    - Goal: Handling TBs of data.
3.  **Choosing a Shard Key**
    - Concepts: Cardinality, Frequency, Monotonically Increasing keys.
    - Goal: Avoid "Jumbo Chunks" and hotspots.

## Module 8: Advanced Tuning & Security

**Goal**: Production readiness.

4.  **Database Profiling**
    - Concepts: Database Profiler, Log levels.
    - Goal: Finding slow queries in production.
5.  **Security Best Practices**
    - Concepts: Authentication (SCRAM/X.509), RBAC (Roles), Encryption at Rest.
    - Goal: Protecting data.
6.  **Backup & Restore**
    - Concepts: `mongodump`, `mongorestore`, Ops Manager.
    - Goal: Disaster recovery.
