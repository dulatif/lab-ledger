# 4. Altering Tables

## 🎯 Learning Goal

Changing the structure later.

## 🧠 Concept

What if you forgot a column?
`ALTER TABLE` allows adding, dropping, or renaming columns.
`TRUNCATE TABLE`: Deletes all rows but keeps the table structure (faster than DELETE).

## 💻 Implementation

```sql
ALTER TABLE users ADD COLUMN email VARCHAR(100);
TRUNCATE TABLE logs; -- Fast wipe
```

## 🧩 Activity / Challenge

1.  Add a `stock_count` column to your imaginary `products` table.

## 🔑 Key Takeaways

- `TRUNCATE` is a DDL command (resets auto-increments in some DBs), unlike `DELETE`.
