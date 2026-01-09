# Lesson 6: Basic Mutations

## Learning Goals

- `mutation` keyword.
- Side effects.

## 1. The Syntax

Syntactically identical to Queries, but implies a write operation.

```graphql
mutation CreatePost {
  createPost(title: "Hello World") {
    id
    title
    createdAt
  }
}
```

## 2. Return Values

Always return the modified data. This allows the client to update its cache immediately without a second fetch.

## 3. Multiple Mutations

If you run multiple mutations in one request, they run **Sequentially** (one after another).
Queries run in **Parallel**.

## Key Takeaways

- Use Mutations for Create, Update, Delete.
- Always ask for the `id` back.
