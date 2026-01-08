# 3. Creating and Dropping Tables

## 🎯 Learning Goal

Building the container.

## 🧠 Concept

**DDL (Data Definition Language)**.

- `CREATE TABLE`: Defines specific columns and types.
- `DROP TABLE`: Deletes the table and _all_ its data permanently.

## 💻 Implementation

```sql
CREATE TABLE users (
    id INT,
    username VARCHAR(50),
    created_at DATE
);

DROP TABLE users;
```

## 🧩 Activity / Challenge

1.  Create a `products` table with `name` and `price`.
2.  Drop it. (Careful in production!).

## 🔑 Key Takeaways

- DDL commands change structure, not content.
