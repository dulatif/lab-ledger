# Lesson 3: High Availability & Clustering

## Learning Goals

- Understand the problem of Automated Failover.
- Explore **Patroni** as the industry standard.
- Understand Connection Pooling (**PgBouncer**).

## 1. The Failover Problem

Streaming Replication gives you a copy. But if the Primary dies at 3 AM:

1.  Who wakes up?
2.  Who promotes the Replica to Primary?
3.  Who tells the Application to connect to the new IP?
4.  How do we prevent "Split Brain" (two servers thinking they are both Primary)?

## 2. Patroni: The HA Standard

**Patroni** is a template for high availability. It runs as a daemon alongside Postgres on every node.

- **DCS (Distributed Consensus Store)**: Patroni relies on **Etcd** or **Consul** to store the cluster state.
- **Leader Election**: Patroni nodes vote. If the leader vanishes, the remaining nodes elect a new leader automatically.
- **Fencing**: It ensures the old leader is demoted (stonith) before promoting a new one to prevent Split Brain.

**Architecture**:
App -> HAProxy (checks Patroni API) -> Primary Postgres Node

## 3. Connection Pooling (PgBouncer)

PostgreSQL has a high per-connection memory cost.

- **Problem**: If you have 5000 serverless functions connecting simultaneously, Postgres will crash (OOM).
- **Solution**: **PgBouncer**.

It sits in front of Postgres.

- App opens 5000 connections to PgBouncer (cheap).
- PgBouncer maintains only 50 connections to Postgres (expensive).
- It routes queries through the available pool connections.

**Modes**:

- **Session Pooling**: Connection kept until client disconnects.
- **Transaction Pooling** (Most Efficient): Connection kept only for duration of `BEGIN...COMMIT`.

## Key Takeaways

- **Don't build your own HA scripts**. Use **Patroni**.
- **Split Brain** destroys data. Consensus stores (Etcd) prevent this.
- **PgBouncer** is mandatory for high-scale applications to save DB memory.
