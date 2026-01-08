# 8. Using GraphQL Variables

## 🎯 Learning Goal

Same as Code First - Client side concept.

## 🧠 Concept

Works exactly the same. The backend doesn't care how the client sends the query (literal vs variable).

## 💻 Implementation

```graphql
query GetCoffee($id: ID!) {
  coffee(id: $id) {
    name
  }
}
```

## 🧩 Activity / Challenge

1.  Verify variables work.

## 🔑 Key Takeaways

- Zero difference here.
