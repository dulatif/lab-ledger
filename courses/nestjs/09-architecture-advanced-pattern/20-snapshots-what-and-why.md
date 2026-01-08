# 20. Snapshots: What & Why?

## 🎯 Learning Goal

Optimize performance when an Aggregate has thousands of events.

## 🧠 Concept

Replaying 10,000 events every time we want to load a user is slow. 🐢
**Snapshotting** is a caching strategy.
Every 100 events, we save the **Current State** to a separate "Snapshot Store".

To load:

1.  Load latest Snapshot (e.g., version 100).
2.  Load events _after_ version 100 (101, 102...).
3.  Apply only the new events.

## 💻 Implementation

Not usually needed for most apps (users rarely change their name 10,000 times).
But essential for high-frequency streams (like Stock Prices or IoT Sensors).

## 🧩 Activity / Challenge

1.  Discuss: When should you implement snapshotting?
    - _Ans_: Only when performance metrics show that `loadFromHistory` is the bottleneck. Don't premature optimize.

## 🔑 Key Takeaways

- **Snapshot**: A checkpoint of state at a specific version.
- It's a cache. If you delete snapshots, you can still rebuild everything from the Event Log (just slower). Protocol: **Safety first**.
