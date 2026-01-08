# 5. Idempotency and Reliability

## 🎯 Learning Goal

Handling network retries safely.

## 🧠 Concept

**Idempotency**: Executing the request N times has same effect as 1 time.

- GET, PUT, DELETE: Inherently idempotent.
- POST: Not idempotent (Charges card N times).

**Idempotency Key**:
Client sends `Idempotency-Key: uuid`.
Server checks Redis. If key exists, return previous response. Do not re-process.

## 💻 Implementation

Crucial for Payments and Messaging.

## 🧩 Activity / Challenge

1.  User clicks "Pay" twice on shaky 4G.
2.  First request hangs. Second succeeds.
3.  First request un-hangs and reaches server.
4.  Without keys -> Double Charge. With keys -> Ignored.

## 🔑 Key Takeaways

- Mandatory for financial APIs.
