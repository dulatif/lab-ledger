# 3. Understanding Joins

## 🎯 Learning Goal

Combining tables. The heart of SQL.

## 🧠 Concept

- **INNER JOIN**: Match in BOTH tables.
- **LEFT JOIN**: All from LEFT table, matches from RIGHT (or NULL).
- **RIGHT JOIN**: All from RIGHT (Rare).
- **FULL OUTER JOIN**: All from BOTH (Match or NULLs).

## 💻 Implementation

```sql
SELECT users.name, orders.amount
FROM users
INNER JOIN orders ON users.id = orders.user_id;
```

## 🧩 Activity / Challenge

1.  Use `LEFT JOIN` to find Users who have _never_ placed an order (`WHERE orders.id IS NULL`).

## 🔑 Key Takeaways

- Visualizing Venn Diagrams helps.
