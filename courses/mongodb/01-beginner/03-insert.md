# Lesson 3: Inserting Documents

## Learning Goals

- `insertOne`.
- `insertMany`.
- `_id`.

## 1. The `_id` Field

Every document MUST have a unique `_id`.
If you don't provide one, MongoDB generates an `ObjectId` automatically.
`ObjectId`: 12-byte unique identifier (Timestamp + Machine ID + Random).

## 2. Insert One

```javascript
db.users.insertOne({
  name: "Alice",
  age: 25,
  skills: ["JS", "Python"],
});
```

## 3. Insert Many

```javascript
db.users.insertMany([
  { name: "Bob", age: 30 },
  { name: "Charlie", age: 35 },
]);
```

## Key Takeaways

- You can mix different shapes in the same collection (but try to be consistent).
- `insertMany` is atomic (ordered by default). If one fails, the rest stop (unless `ordered: false`).
