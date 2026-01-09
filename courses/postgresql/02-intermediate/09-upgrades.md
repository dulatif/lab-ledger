# Lesson 9: Upgrades

## Learning Goals

- Distinguish between Minor and Major upgrades.
- Perform a Major Version upgrade using `pg_upgrade`.
- Understand Zero-Downtime upgrades (Logical Replication).

## 1. Versioning

PostgreSQL uses `Major.Minor` versioning.

- **15.2 -> 15.3**: Minor. Just bug fixes. **Binary Compatible**.
  - _Action_: Stop server, install new binary, start server. Done.
- **14.5 -> 15.0**: Major. New features. Internal storage format changes. **NOT Compatible**.
  - _Action_: Requires a migration strategy.

## 2. Using `pg_upgrade` (In-Place)

The standard tool for major upgrades.

**Mechanism**:
It doesn't copy the data. It migrates the _metadata_ files to the new format and "links" the old data files to the new cluster.

- **Speed**: Near instant (minutes), even for Terabyte-sized databases, if using hard links (`--link`).

**Steps**:

1.  Install both old (v14) and new (v15) binaries.
2.  Initialize a new empty v15 cluster.
3.  Run:
    ```bash
    /usr/lib/postgresql/15/bin/pg_upgrade \
      -b /usr/lib/postgresql/14/bin \
      -B /usr/lib/postgresql/15/bin \
      -d /var/lib/postgresql/14/data \
      -D /var/lib/postgresql/15/data \
      --link
    ```

**Risk**: If it fails halfway, and you used `--link`, you might corrupt the old cluster. **Always take a backup first.**

## 3. Logical Replication (Zero-Downtime)

For mission-critical apps that cannot afford even the 10 minutes of downtime for `pg_upgrade`.

1.  Set up the new V15 server empty.
2.  Configure **Logical Replication** (Publisher = V14, Subscriber = V15).
3.  Let the data sync (Initial copy + Change Data Capture).
4.  When caught up, switch the application URL to the new server.

## Key Takeaways

- **Minor updates** are easy (restart); **Major updates** are hard (migration).
- **pg_upgrade --link** is the standard for fast, in-place upgrades.
- **Logical Replication** allows for Blue/Green deployments with minimal downtime.
- **ALWAYS BACKUP** before a major upgrade.
