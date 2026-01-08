# 10. Pivot and Unpivot Operations

## 🎯 Learning Goal

Reshaping data.

## 🧠 Concept

**Pivot**: Rows TO Columns. (Monthly Sales per Row -> Jan, Feb, Mar columns).
**Unpivot**: Columns TO Rows.

## 💻 Implementation

Standard SQL `CASE` allows manual pivoting:

```sql
SELECT
  SUM(CASE WHEN month='Jan' THEN amt END) as Jan,
  SUM(CASE WHEN month='Feb' THEN amt END) as Feb
FROM sales;
```

## 🧩 Activity / Challenge

1.  Often easier to do in Excel/Pandas after fetching raw data.

## 🔑 Key Takeaways

- Great for dashboards.
