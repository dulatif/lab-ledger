# 13. Adding Update and Delete Operations

## 🎯 Learning Goal

Complete CRUD.

## 🧠 Concept

Update SDL:

```graphql
input UpdateCoffeeInput {
  name: String
  brand: String
}

type Mutation {
  updateCoffee(id: ID!, input: UpdateCoffeeInput!): Coffee!
  removeCoffee(id: ID!): Coffee!
}
```

## 💻 Implementation

Implement Service methods.

## 🧩 Activity / Challenge

1.  Reflect changes in SDL.
2.  Implement Logic.
3.  Test.

## 🔑 Key Takeaways

- SDL forces you to document the API before building it.
