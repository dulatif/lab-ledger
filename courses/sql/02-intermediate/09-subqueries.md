# 9. Writing Nested Subqueries

## 🎯 Learning Goal

Queries inside Queries.

## 🧠 Concept

Can be in SELECT, FROM, or WHERE.
**Nested**: Run inner, pass result to outer.
**Correlated**: Inner query references outer row (Runs for _each_ row -> Slow).

## 💻 Implementation

```sql
-- Find users who spent more than average
SELECT * FROM users
WHERE total_spend > (SELECT AVG(total_spend) FROM users);
```

## 🧩 Activity / Challenge

1.  Find products that have never been ordered.
    - `WHERE id NOT IN (SELECT product_id FROM orders)`.

## 🔑 Key Takeaways

- Commonly replaced by JOINs for performance.
