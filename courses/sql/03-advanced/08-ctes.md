# 8. CTEs and Recursive Queries

## 🎯 Learning Goal

Readable complex queries.

## 🧠 Concept

**CTE (Common Table Expression)**: `WITH` clause. Like a temporary named variable for a query.

**Recursive CTE**: References itself. Useful for Trees (Org chart, Categories).

## 💻 Implementation

```sql
WITH regional_sales AS (
    SELECT region, SUM(amount) as total FROM orders GROUP BY region
)
SELECT * FROM regional_sales WHERE total > 1000;
```

## 🧩 Activity / Challenge

1.  Write a recursive CTE to generate numbers 1 to 10.

## 🔑 Key Takeaways

- CTEs make spaghetti SQL readable.
