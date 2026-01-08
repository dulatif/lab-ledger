# 19. Inbox pattern

## 🎯 Learning Goal

Handle **Idempotency** (Processing the same message twice).

## 🧠 Concept

Due to "At-Least-Once" delivery (Lesson 16), your consumer might receive "Order Created #55" twice.
If you blindly insert into DB, you might get "Duplicate Key Error" (Good) or create two shipping labels (Bad).

**Inbox Pattern**:

1.  Receiver gets message.
2.  Checks `inbox` table: "Have I processed MessageID 123?"
3.  If yes, Ignore (Ack immediately).
4.  If no, Process + Insert into `inbox` table.

## 💻 Implementation

Often combined with Outbox. The Message ID generated in the Producer's Outbox is tracked in the Consumer's Inbox.

## 🧩 Activity / Challenge

1.  This is mostly conceptual for this course level.
2.  Understand that "Exactly Once" delivery is mathematically impossible in distributed systems. We fake it using "Deduplication" (Inbox).

## 🔑 Key Takeaways

- **Idempotency** IS REQUIRED for any safe microservice.
- `stripe.charge({ idempotencyKey: ... })` is a famous example.
