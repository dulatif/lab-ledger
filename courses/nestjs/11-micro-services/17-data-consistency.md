# 17. Data Consistency

## 🎯 Learning Goal

Address the "Dual Write" problem.

## 🧠 Concept

Common Bug:

```typescript
await this.ordersRepo.save(order); // 1. Save to DB
await this.client.emit("order_created", order); // 2. Send Event
```

What if Step 2 fails? (Broker down).

- We have an Order in DB.
- But "Shipping Service" never knows about it.
- Shipping never happens. 😱

This breaks **Consistency**.

## 💻 Implementation

We need a way to make [DB Save + Broker Publish] atomic.
Two main patterns: **2PC (Two-Phase Commit)** - Slow, hard.
**Transactional Outbox** - The standard solution.

## 🧩 Activity / Challenge

1.  Try to reproduce the bug.
2.  Stop RabbitMQ container.
3.  Create an Order.
4.  Start RabbitMQ.
5.  Notice the event is lost.

## 🔑 Key Takeaways

- Distributed Systems cannot rely on ACID across services. We rely on **Eventual Consistency**.
