# Lesson 5: Updating & Deleting

## Learning Goals

- `updateOne`, `updateMany`.
- Update Operators (`$set`, `$inc`, `$push`).
- `deleteOne`.

## 1. Updating

Requires two arguments: **Filter** (Which docs?) and **Update** (What changes?).
**NEVER** use `update()` (legacy). Use `updateOne` or `updateMany`.

```javascript
// Set a field
db.users.updateOne({ name: "Alice" }, { $set: { age: 26 } });

// Increment a number
db.products.updateMany(
  {}, // All docs
  { $inc: { views: 1 } }
);
```

## 2. Array Operators

- `$push`: Add to array.
- `$pull`: Remove from array.
- `$addToSet`: Add only if unique.

## 3. Deleting

`db.users.deleteOne({ _id: ... })`.
Be careful with `deleteMany({})` -> Deletes everything!

## Key Takeaways

- Updates are "In-Place" (efficient).
- `$set` is critical. If you pass `{ age: 26 }` without `$set` (in old drivers), it might replace the _whole document_.
