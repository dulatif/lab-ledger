# Lesson 3: Lookup (Joins)

## Learning Goals

- `$lookup`.
- Left Outer Join.

## 1. The `$lookup` Stage

Pull data from another collection.

```javascript
{
  $lookup: {
    from: "orders",       // Target collection
    localField: "_id",    // Field in current (User)
    foreignField: "userId", // Field in target (Order)
    as: "orderHistory"    // Output array field name
  }
}
```

## 2. Performance Implications

`$lookup` is slow compared to Embedding.
It runs a query for every document in the pipeline (or optimized batch).
Avoid detailed lookups on massive datasets if possible.

## Key Takeaways

- Result is always an **Array** (even if 1 match).
- Typically followed by `$unwind` if you expect 1:1 match.
