# Lesson 2: Sharding

## Learning Goals

- Horizontal Scaling.
- Architecture.

## 1. Why Sharding?

Vertical Scaling (Bigger RAM/CPU) has limits.
Horizontal Scaling (More machines) is theoretically infinite.
MongoDB distributes data across multiple Replica Sets (Shards).

## 2. Components

- **Shard**: A Replica Set holding a subset of data.
- **Mongos**: The Router. Application connects here. It looks like a normal DB, but it routes queries to the right shard.
- **Config Servers**: Stores metadata (Which chunk is on which shard?).

## 3. Chunks

Data is split into 64MB blocks called "Chunks".
Balancer processes migrate chunks between shards to ensure even distribution.

## Key Takeaways

- Sharding is complex. Don't do it unless you have >2TB of data or massive write throughput.
