# 2. Grouping Results (GROUP BY, HAVING)

## 🎯 Learning Goal

Summarizing per category.

## 🧠 Concept

**GROUP BY**: Buckets rows based on a column.
**HAVING**: Filters _after_ the aggregation (WHERE filters _before_).

## 💻 Implementation

```sql
SELECT category, COUNT(*) as count
FROM products
GROUP BY category
HAVING COUNT(*) > 5;
```

(Find categories with more than 5 products).

## 🧩 Activity / Challenge

1.  Try to use `WHERE COUNT(*) > 5`. It fails. You must use `HAVING`.

## 🔑 Key Takeaways

- Order: WHERE -> GROUP BY -> HAVING.
