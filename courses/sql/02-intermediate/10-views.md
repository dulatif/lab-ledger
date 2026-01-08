# 10. Working with Views

## 🎯 Learning Goal

Saving a query as a virtual table.

## 🧠 Concept

**View**: Stored query. Does not store data (usually).
Simplifies complex joins for other users.
`CREATE VIEW active_users AS ...`

## 💻 Implementation

```sql
CREATE VIEW report_sales_2023 AS
SELECT ... FROM ... WHERE year = 2023;

-- Usage
SELECT * FROM report_sales_2023;
```

## 🧩 Activity / Challenge

1.  Create a view that hides sensitive columns (like salary) so non-admins can query employees.

## 🔑 Key Takeaways

- Great for security and abstraction.
