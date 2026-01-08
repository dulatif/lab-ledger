# 8. Passing Arguments: Returning a Coffee by ID

## 🎯 Learning Goal

Accept input from the client query.

## 🧠 Concept

In REST: `GET /coffees/:id` -> `@Param('id')`.
In GraphQL: `query { coffee(id: 1) { ... } }` -> `@Args('id')`.

## 💻 Implementation

```typescript
@Query(returns => Coffee, { nullable: true }) // Coffee can be null if not found
async coffee(@Args('id', { type: () => Int }) id: number) {
  // Logic to find coffee by id
  return { id, name: 'Shipwreck Roast', brand: 'Buddy Brew' };
}
```

## 🧩 Activity / Challenge

1.  Add the `coffee` query to your resolver.
2.  Use Playground:
    ```graphql
    query {
      coffee(id: 1) {
        name
      }
    }
    ```
3.  Try omitting the argument (Error).

## 🔑 Key Takeaways

- `@Args()` extracts arguments by name.
- We specify the type explicitly (`{ type: () => Int }`) to help NestJS generate correct schema map if TS metadata implies `number` (which is Float by default).
