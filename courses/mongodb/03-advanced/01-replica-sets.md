# Lesson 1: Replica Sets

## Learning Goals

- High Availability.
- Oplog.
- Read Preference.

## 1. Architecture

- **Primary**: Receives all Writes. Replicates to Secondaries.
- **Secondary**: Replicates data. Can serve Reads (Eventual Consistency).
- **Arbiter**: Participates in elections but holds no data.

## 2. Failover

If Primary dies, an Election is held. A Secondary becomes the new Primary.
Automatic and usually takes <2 seconds.

## 3. The Oplog (Operations Log)

A special capped collection that keeps a rolling record of all operations that modify data.
Secondaries tail the Primary's Oplog.

## Key Takeaways

- Never run a standalone node in production. Always use a Replica Set (min 3 nodes).
- `ReadPreference: secondaryPreferred` can offload read traffic.
