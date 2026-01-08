# 10. Using GraphQL Variables

## 🎯 Learning Goal

Write reusable queries on the client.

## 🧠 Concept

Hardcoding strings (`name: "Latte"`) in queries is bad.
We use Variables (`$name`).

## 💻 Implementation

This is a Client-side (Playground) concept.

**Query**:

```graphql
mutation CreateMyCoffee($payload: CreateCoffeeInput!) {
  createCoffee(createCoffeeInput: $payload) {
    name
    id
  }
}
```

**Variables JSON**:

```json
{
  "payload": {
    "name": "Mocha",
    "brand": "Nest",
    "flavors": ["Chocolate"]
  }
}
```

## 🧩 Activity / Challenge

1.  Try the above in the Playground.
2.  Notice how much cleaner the query window is.

## 🔑 Key Takeaways

- Variables prevent injection attacks and allow caching of query strings.
