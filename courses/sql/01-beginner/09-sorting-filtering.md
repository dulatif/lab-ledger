# 9. Sorting and Filtering (ORDER BY, DISTINCT)

## 🎯 Learning Goal

Organizing output.

## 🧠 Concept

- **ORDER BY**: Sorts result. `ASC` (default) or `DESC`.
- **DISTINCT**: Removes duplicate rows from result.
- **LIMIT**: Restrict number of rows.

## 💻 Implementation

```sql
-- Top 5 most expensive products
SELECT DISTINCT category, price
FROM products
ORDER BY price DESC
LIMIT 5;
```

## 🧩 Activity / Challenge

1.  If you have 5 users named "Alice", `SELECT DISTINCT name` returns 1 row.

## 🔑 Key Takeaways

- The order of operations is specific: WHERE -> ORDER BY -> LIMIT.
