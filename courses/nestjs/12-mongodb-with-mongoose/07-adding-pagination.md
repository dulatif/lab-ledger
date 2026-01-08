# 7. Adding Pagination

## 🎯 Learning Goal

Handle large datasets efficiently.

## 🧠 Concept

`find()` returns everything. Bad for 1M records.
We use `skip()` (offset) and `limit()` (take).

## 💻 Implementation

```typescript
findAll(paginationQuery: PaginationQueryDto) {
  const { limit, offset } = paginationQuery;
  return this.coffeeModel
    .find()
    .skip(offset)
    .limit(limit)
    .exec();
}
```

## 🧩 Activity / Challenge

1.  Add 10 coffees.
2.  Request `?limit=2&offset=0`.
3.  Request `?limit=2&offset=2`.
4.  Observe the results.

## 🔑 Key Takeaways

- It's surprisingly simple in Mongo.
- For massive scale, avoid `skip` (Cursor-based pagination is better), but `skip` is fine for most apps.
