# 4. Advanced Joins (Self, Cross)

## 🎯 Learning Goal

Exotic join types.

## 🧠 Concept

**Self Join**: Joining a table to itself (e.g., Employees table with `manager_id` pointing to `employee_id`).
**Cross Join**: Cartesian Product. Every row matched with Every row. (Multiplication).

## 💻 Implementation

```sql
-- Self Join
SELECT e.name as Employee, m.name as Manager
FROM employees e
JOIN employees m ON e.manager_id = m.id;
```

## 🧩 Activity / Challenge

1.  If Table A has 10 rows and Table B has 10 rows, how many rows does Cross Join produce?
    - 100 rows.

## 🔑 Key Takeaways

- Cross Join is accidental 99% of the time (Missing ON clause). Be careful.
