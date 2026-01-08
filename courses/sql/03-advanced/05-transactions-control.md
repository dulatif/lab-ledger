# 5. Controlling Transactions

## 🎯 Learning Goal

Applying ACID.

## 🧠 Concept

- `BEGIN / START TRANSACTION`.
- `COMMIT`: Save changes.
- `ROLLBACK`: Undo changes since BEGIN.

## 💻 Implementation

```sql
BEGIN;
UPDATE accounts SET bal = bal - 100 WHERE id = 1;
UPDATE accounts SET bal = bal + 100 WHERE id = 2;
-- If error here
ROLLBACK;
-- Else
COMMIT;
```

## 🧩 Activity / Challenge

1.  Open two terminal windows.
2.  Start transaction in one. Update row.
3.  Try to read row in other. (It might wait or see old data depending on Isolation Level).

## 🔑 Key Takeaways

- Essential for data consistency.
