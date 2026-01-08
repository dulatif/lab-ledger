# 3. Optimization Strategies

## 🎯 Learning Goal

Writing faster SQL.

## 🧠 Concept

1.  **Selectivity**: Index high-cardinality columns (Email vs Gender).
2.  **SARGable**: Search ARGument ABLE.
    - `WHERE YEAR(date) = 2023` -> Bad (Function on column prevents index).
    - `WHERE date >= '2023-01-01'` -> Good.
3.  **Joins vs Subqueries**: Joins are generally better optimized by engines.

## 💻 Implementation

Avoid wildcard at start: `LIKE '%abc'` cannot use index. `LIKE 'abc%'` can.

## 🧩 Activity / Challenge

1.  Refactor `SELECT *` to select only needed columns to reduce network IO.

## 🔑 Key Takeaways

- Write queries to help the optimizer.
