# 6. GraphQL Schemas, Types, and Scalars

## 🎯 Learning Goal

Define types in SDL.

## 🧠 Concept

Same types available: `Int`, `Float`, `String`, `Boolean`, `ID`.
We define them in text.

```graphql
type Coffee {
  id: ID! # Non-nullable
  description: String # Nullable
}
```

## 💻 Implementation

Update `coffees.graphql`.

## 🧩 Activity / Challenge

1.  Make description nullable.
2.  Query it.

## 🔑 Key Takeaways

- SDL is language-neutral. You could implement this in Python or Go later.
