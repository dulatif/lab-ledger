# 5. Ensuring Data Integrity with Constraints

## 🎯 Learning Goal

Rules for your data.

## 🧠 Concept

- **PRIMARY KEY (PK)**: Unique ID for the row. Not Null.
- **FOREIGN KEY (FK)**: Links to a PK in another table. Enforces relationship.
- **UNIQUE**: No duplicates (e.g., Email).
- **NOT NULL**: Must have a value.
- **CHECK**: Custom rule (`price > 0`).

## 💻 Implementation

```sql
CREATE TABLE users (
    id INT PRIMARY KEY,
    email VARCHAR(100) UNIQUE NOT NULL
);
```

## 🧩 Activity / Challenge

1.  Try to insert a duplicate email into the table above. The DB will reject it.

## 🔑 Key Takeaways

- Constraints are the "Safety Net" of the database.
