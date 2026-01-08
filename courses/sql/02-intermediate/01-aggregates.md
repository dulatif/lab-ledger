# 1. Aggregate Functions

## 🎯 Learning Goal

Summarizing data.

## 🧠 Concept

Taking many rows and returning one value.

- `COUNT(*)`: Count rows.
- `SUM(col)`: Total.
- `AVG(col)`: Average.
- `MIN/MAX(col)`: Extremes.

## 💻 Implementation

```sql
SELECT COUNT(*) FROM users;
SELECT AVG(price) FROM products WHERE category = 'Electronics';
```

## 🧩 Activity / Challenge

1.  `COUNT(email)` vs `COUNT(*)`?
    - `COUNT(col)` ignores NULLs. `COUNT(*)` counts row existence.

## 🔑 Key Takeaways

- Aggregates usually collapse the result set to a single row unless utilized with GROUP BY.
