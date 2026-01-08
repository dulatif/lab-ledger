# 4. Creating our first GraphQL Application

## 🎯 Learning Goal

Create the first `.graphql` file.

## 🧠 Concept

We need a root Query.

## 💻 Implementation

Create `src/coffees/coffees.graphql`:

```graphql
type Coffee {
  id: ID!
  name: String!
  brand: String!
  flavors: [String!]!
}

type Query {
  coffees: [Coffee!]!
}
```

## 🧩 Activity / Challenge

1.  Run the app.
2.  It should start now!
3.  Check Playground.

## 🔑 Key Takeaways

- The SDL is the source of truth.
- `ID` is a scalar unique identifier (strings usually).
