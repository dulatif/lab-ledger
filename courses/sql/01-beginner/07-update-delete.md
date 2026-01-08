# 7. Modifying and Deleting Data (UPDATE, DELETE)

## 🎯 Learning Goal

Changing existing records.

## 🧠 Concept

**UPDATE**: Change values. **ALWAYS USE WHERE**.
**DELETE**: Remove rows. **ALWAYS USE WHERE**.

## 💻 Implementation

```sql
UPDATE users SET name = 'Ali' WHERE id = 1;

DELETE FROM users WHERE id = 3;
```

## 🧩 Activity / Challenge

1.  **Safety Check**: If you run `DELETE FROM users;` (no WHERE), you wipe the entire table.
2.  Always write the `SELECT * FROM ... WHERE ...` first to check what you are about to delete.

## 🔑 Key Takeaways

- With great power comes great responsibility.
