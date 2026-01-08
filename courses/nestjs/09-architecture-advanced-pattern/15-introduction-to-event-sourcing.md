# 15. Introduction to Event Sourcing

## 🎯 Learning Goal

Understand the radical shift from storing "Current State" to storing "Everything that happened".

## 🧠 Concept

**State-based persistence (Standard):**
We update the row. History is lost.

- `UPDATE accounts SET balance = 100 WHERE id = 1`

**Event Sourcing:**
We insert a new row. History is the Source of Truth.

- `INSERT INTO events (type, payload) VALUES ('AccountOpened', { amount: 0 })`
- `INSERT INTO events (type, payload) VALUES ('Deposited', { amount: 100 })`

To get the current balance, we replay all events:
`0 + 100 = 100`

## 💻 Implementation

There is no "Update" in Event Sourcing. Only "Append".
Your database becomes an append-only log tailored for specific Aggregates.

## 🧩 Activity / Challenge

1.  Bank Account analogy: Your bank statement _is_ an Event Log. The "Current Balance" is just a projection calculated from that log.
2.  Think: Why is this powerful?
    - Audit Trail is free.
    - Debugging: You can replay traffic to see exactly how a user got into a bad state.
    - Temporal Querying: "What was the system state last Tuesday?" Replay events only up to Tuesday.

## 🔑 Key Takeaways

- **Source of Truth**: The Event Log.
- **Projection**: The current state (derived from log).
- **Immutability**: You never delete or change an event. You only add "Correction" events.
