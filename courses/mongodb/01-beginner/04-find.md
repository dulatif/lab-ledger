# Lesson 4: Finding Documents

## Learning Goals

- `find`.
- Query Operators.
- Projections.

## 1. Basic Find

- `db.users.find()`: Select \* from users.
- `db.users.find({ name: "Alice" })`: Select \* from users where name = 'Alice'.

## 2. Query Operators

- `$eq`: Equal (Default).
- `$gt` / `$lt`: Greater Than / Less Than. `find({ age: { $gt: 18 } })`.
- `$in`: In array. `find({ status: { $in: ["A", "B"] } })`.
- `$and` / `$or`: Logical operators.

## 3. Projection

Return only specific fields (like `SELECT name, age`).

```javascript
db.users.find({}, { name: 1, _id: 0 }); // Show name, hide _id
```

## Key Takeaways

- `find()` returns a **Cursor** (pointer to data), not the data itself.
- Projections save network bandwidth.
