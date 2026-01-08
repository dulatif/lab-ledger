# 1. Before we get started

## 🎯 Learning Goal

Prepare your mindset for GraphQL.

## 🧠 Concept

GraphQL is a query language for APIs.
Unlike REST (multiple endpoints), GraphQL exposes a **Single Endpoint**.
The Client asks for exactly what it needs:

```graphql
query {
  user(id: 1) {
    name
    posts {
      title
    }
  }
}
```

No Over-fetching (getting too much data).
No Under-fetching (need to make 3 calls to get related data).

## 💻 Implementation

We are going to build a "Coffee API" again, but this time using GraphQL.

## 🧩 Activity / Challenge

1.  If you know REST, forget about `@Get()`, `@Post()`.
2.  Think in **Graphs**: Nodes (Types) and Edges (Relationships).

## 🔑 Key Takeaways

- NestJS fully supports GraphQL via `@nestjs/graphql` wrapper on top of Apollo Server or Mercurius.
