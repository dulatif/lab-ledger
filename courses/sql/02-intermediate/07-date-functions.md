# 7. Date and Time Functions

## 🎯 Learning Goal

Temporal logic.

## 🧠 Concept

Syntax varies heavily by DB (Postgres vs MySQL vs SQL Server).

- `NOW()` or `CURRENT_TIMESTAMP`.
- `DATE_ADD` or `interval`.
- `DATEDIFF`: Time between two dates.

## 💻 Implementation

```sql
-- Postgres style
SELECT * FROM orders
WHERE created_at > NOW() - INTERVAL '7 days';
```

## 🧩 Activity / Challenge

1.  Find all users born in 1990.
    - `WHERE EXTRACT(YEAR FROM dob) = 1990`.

## 🔑 Key Takeaways

- Always store dates in UTC. Convert to timezone on display.
