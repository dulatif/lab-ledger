# 6. Adding Data (INSERT)

## 🎯 Learning Goal

DML (Data Manipulation Language) starts here.

## 🧠 Concept

`INSERT INTO` ... `VALUES`.
You can insert single or multiple rows.

## 💻 Implementation

```sql
INSERT INTO users (id, name) VALUES (1, 'Alice');

-- Multiple
INSERT INTO users (id, name) VALUES
(2, 'Bob'),
(3, 'Charlie');
```

## 🧩 Activity / Challenge

1.  What happens if you omit the `id` column but it's a Primary Key? (Error, unless Auto-Increment is on).

## 🔑 Key Takeaways

- Always list columns explicitly `INSERT INTO table (col1, col2)` to be safe against schema changes.
