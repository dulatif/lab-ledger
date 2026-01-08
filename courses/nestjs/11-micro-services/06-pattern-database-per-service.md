# 6. Pattern: Database per service

## 🎯 Learning Goal

Enforce strict data sovereignty.

## 🧠 Concept

Anti-Pattern: **Shared Database**.

- Service A writes to Table X.
- Service B reads from Table X.
- Service A changes schema of X. Service B crashes. 💥
- This is a "Distributed Monolith".

Pattern: **Database per Service**.

- Orders Service -> has its own `orders_db`.
- Users Service -> has its own `users_db`.
- If Orders needs User data, it must call the Users Service API or listen to events to build a local replica.

## 💻 Implementation

In `docker-compose.yml`, you define multiple databases or schemas.

## 🧩 Activity / Challenge

1.  Why is this painful? Joins are impossible!
2.  You cannot do `SELECT * FROM orders JOIN users ...`.
3.  You must fetch Order, get `userId`, then fetch User. (N+1 problem happens at HTTP level).

## 🔑 Key Takeaways

- Strict bounds prevent coupling.
- Data duplication (caching other service's data) is acceptable and encouraged to improve performance/autonomy.
