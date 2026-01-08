# 7. Window Functions

## 🎯 Learning Goal

Analytics superpower.

## 🧠 Concept

Aggregates (SUM) collapse rows.
**Window Functions** calculate across a set of rows _related_ to current row, without collapsing.
"Running Total", "Rank within Department".

## 💻 Implementation

```sql
SELECT name, dept,
  RANK() OVER (PARTITION BY dept ORDER BY salary DESC) as rank
FROM employees;
```

Returns every employee, with their rank in their dept.

## 🧩 Activity / Challenge

1.  Calculate Running Total of sales by day.
    - `SUM(amount) OVER (ORDER BY date)`.

## 🔑 Key Takeaways

- Extremely powerful for reporting.
