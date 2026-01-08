# 9. Manipulating Data with Mutations

## 🎯 Learning Goal

Write inputs in SDL.

## 🧠 Concept

SDL uses `input` keyword.

```graphql
input CreateCoffeeInput {
  name: String!
  brand: String!
  flavors: [String!]!
}

type Mutation {
  createCoffee(input: CreateCoffeeInput!): Coffee!
}
```

## 💻 Implementation

Resolver:

```typescript
@Mutation()
async createCoffee(@Args('input') input: CreateCoffeeInput) {
  // ...
}
```

## 🧩 Activity / Challenge

1.  Add `input` and `Mutation` to SDL.
2.  Implement resolver.
3.  Use generated `CreateCoffeeInput` class/interface.

## 🔑 Key Takeaways

- The `definitions` generator is crucial here, otherwise you are typing `any`.
