# 18. Transactional Outbox Pattern

## 🎯 Learning Goal

Implement Atomic messaging.

## 🧠 Concept

Instead of publishing to RabbitMQ directly:

1.  Start DB Transaction.
2.  Save Order to `orders` table.
3.  Save Message to `outbox` table (in the SAME transaction).
4.  Commit Transaction. (Atomic! Both happen or neither).

Then, a background **Relay Process** (Cron job or separate worker) polls the `outbox` table and publishes to RabbitMQ. If it fails, it retries.

## 💻 Implementation

You can build this manually or use a library like `massive-js` or just TypeORM.

```typescript
await request.manager.transaction(async t => {
   const order = await t.save(Order, ...);
   await t.save(OutboxMessage, {
      topic: 'order_created',
      payload: order
   });
});
```

## 🧩 Activity / Challenge

1.  Create an entity `Outbox`.
2.  Refactor `createOrder` to save to both tables.
3.  Write a `Cron()` job that reads `Outbox`, emits to RMQ, and deletes the row interactively.

## 🔑 Key Takeaways

- **Guaranteed Delivery**: This pattern essentially uses your DB as a reliable queue for the first hop.
