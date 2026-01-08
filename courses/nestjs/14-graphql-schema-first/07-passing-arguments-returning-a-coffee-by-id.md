# 7. Passing Arguments: Returning a Coffee by ID

## 🎯 Learning Goal

Handle arguments in SDL.

## 🧠 Concept

Update SDL:

```graphql
type Query {
  coffees: [Coffee!]!
  coffee(id: ID!): Coffee
}
```

## 💻 Implementation

Update Resolver:

```typescript
@Query('coffee') // explicit name if method name differs
async findOne(@Args('id') id: string) {
  return { id, name: 'Latte' };
}
```

## 🧩 Activity / Challenge

1.  Update SDL.
2.  Update Resolver.
3.  Playground: `query { coffee(id: "1") { name } }`.

## 🔑 Key Takeaways

- Note `ID` usually comes as `string` in JS.
