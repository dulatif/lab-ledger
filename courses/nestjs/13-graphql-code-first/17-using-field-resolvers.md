# 17. Using Field Resolvers

## 🎯 Learning Goal

Compute properties or resolve relations lazily.

## 🧠 Concept

If `flavors` are not loaded by default (lazy), we can resolve them only when requested.
**Field Resolver**: A method that runs ONLY when the field is requested.

## 💻 Implementation

Remove `flavors` from the main `@Query` return (if strictly separating).
Or, add a computed field:

```typescript
@ResolveField()
async flavors(@Parent() coffee: Coffee) {
  // Use coffee.id to fetch flavors
  return this.flavorsService.findByCoffeeId(coffee.id);
}
```

## 🧩 Activity / Challenge

1.  Add a `type: 'arabica'` default field via `@ResolveField` that isn't in DB.
2.  Check Playground.

## 🔑 Key Takeaways

- The **N+1 Problem**: If you fetch 100 coffees, and each calls the flavor DB, you trigger 101 queries.
- We fix this later with DataLoaders.
