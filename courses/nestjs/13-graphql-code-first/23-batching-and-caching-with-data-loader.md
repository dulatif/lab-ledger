# 23. Batching and Caching with Data Loader

## 🎯 Learning Goal

Solve the N+1 problem. The most vital performance lesson.

## 🧠 Concept

If I request 100 coffees and their flavors, I don't want 101 queries.
I want 2 queries:

1. `SELECT * FROM coffees`
2. `SELECT * FROM flavors WHERE coffee_id IN (1, 2, ... 100)`

**DataLoader** batches these requests into one "tick" of the event loop.

## 💻 Implementation

1.  Create `FlavorsByCoffeeLoader`.
2.  Inject it into the Field Resolver.

```typescript
// Conceptual
loader.load(1);
loader.load(2);
// ... Data Loader coalesces this into loadMany([1, 2])
```

## 🧩 Activity / Challenge

This is complex code.

1.  Install `dataloader`.
2.  Create a NestJS Module that provides the loader.
3.  Use it in `coffees.resolver.ts`.

## 🔑 Key Takeaways

- Always use DataLoaders for relations in GraphQL.
- Without them, your API will die under load.
