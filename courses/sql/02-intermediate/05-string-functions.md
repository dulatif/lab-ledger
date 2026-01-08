# 5. String Manipulation

## 🎯 Learning Goal

Cleaning text in SQL.

## 🧠 Concept

- `CONCAT(a, b)`: Join strings.
- `SUBSTRING(str, start, len)`: Extract part.
- `UPPER/LOWER`: Case conversion.
- `TRIM`: Remove whitespace.

## 💻 Implementation

```sql
SELECT CONCAT(first_name, ' ', last_name) as full_name
FROM users;
```

## 🧩 Activity / Challenge

1.  Format email to always be lowercase on Insert?
    - `INSERT INTO ... VALUES (LOWER('Bob@Example.com'))`.

## 🔑 Key Takeaways

- Useful for formatting reports directly from DB.
