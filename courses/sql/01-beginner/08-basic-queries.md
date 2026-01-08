# 8. Basic Queries (SELECT, FROM, WHERE)

## 🎯 Learning Goal

asking questions.

## 🧠 Concept

- `SELECT`: Which columns? (`*` for all).
- `FROM`: Which table?
- `WHERE`: Which rows? (Filter).

Operators: `=`, `<>`, `>`, `<`, `LIKE` (Pattern matching), `IN` (List).

## 💻 Implementation

```sql
SELECT name, email
FROM users
WHERE age > 18 AND name LIKE 'A%';
```

## 🧩 Activity / Challenge

1.  Find all users named "Alice" OR "Bob".
    - `WHERE name IN ('Alice', 'Bob')`.

## 🔑 Key Takeaways

- `SELECT *` is good for debugging, bad for production (performance).
