# Lesson 3: Choosing a Shard Key

## Learning Goals

- Cardinality.
- Hotspots.

## 1. The Shard Key

The field used to distribute documents. **Immutable**.
Once chosen, you cannot change it (easily).

## 2. Bad Keys

- **Monotonically Increasing (e.g., Timestamp, ObjectId)**: All new writes go to the _last_ chunk (Hotspot). One shard takes 100% load.
- **Low Cardinality (e.g., Boolean, Gender)**: Only 2 chunks possible. Cannot scale beyond 2 shards.

## 3. Good Keys

- High Cardinality (e.g., `userId`, `deviceId`).
- Random Distribution (e.g., Hashed Index).

## Key Takeaways

- Selecting the wrong shard key can kill your cluster.
- Hashed Sharding is the safest default for even distribution.
