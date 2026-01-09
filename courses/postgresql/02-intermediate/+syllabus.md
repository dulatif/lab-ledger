# PostgreSQL Intermediate Course Syllabus

## Module 3: Security & Access Control

**Goal**: Secure the database and manage user access effectively.

1.  **Authentication & `pg_hba.conf`**
    - Concepts: Host-based authentication, Peer/md5/SCRAM, Listen addresses.
    - Goal: Control who can connect to the database server.
2.  **Authorization & Roles**
    - Concepts: Roles vs Users, GRANT/REVOKE, Default Privileges, Groups.
    - Goal: Manage what users can do once connected.
3.  **Advanced Security**
    - Concepts: Row-Level Security (RLS), SSL/TLS, Data Anonymization.
    - Goal: Implement granular access control and encryption.

## Module 4: Backup & Recovery

**Goal**: Implement comprehensive backup strategies to prevent data loss.

4.  **Logical Backups**
    - Concepts: `pg_dump`, `pg_dumpall`, `pg_restore`, Custom formats.
    - Goal: Backup and restore individual databases or objects.
5.  **Physical Backups & PITR**
    - Concepts: `pg_basebackup`, Point-in-Time Recovery (PITR), WAL Archiving.
    - Goal: Perform file-level backups and restore to a specific momenbt.
6.  **Enterprise Tools Comparison**
    - Concepts: Architecture of Barman vs. pgBackRest vs. WAL-G.
    - Goal: specific deep dive into `pgbackrest` setup and basic usage.
    - _Note: Focusing on pgBackRest as the industry standard, while comparing features._

## Module 5: Maintenance & Configuration

**Goal**: Keep the database healthy and performant through routine maintenance.

7.  **Vacuum & Bloat**
    - Concepts: Dead tuples, Autovacuum tuning, VACUUM FULL.
    - Goal: Understand and manage database bloat.
8.  **Configuration Tuning**
    - Concepts: `postgresql.conf`, Memory settings (shared_buffers, work_mem), WAL settings.
    - Goal: Tune essential parameters for hardware.
9.  **Upgrades**
    - Concepts: Minor vs Major upgrades, `pg_upgrade`, Logical Replication for zero-downtime.
    - Goal: Keep PostgreSQL up to date safely.
