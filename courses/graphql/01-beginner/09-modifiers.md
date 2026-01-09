# Lesson 9: Lists & Non-Null

## Learning Goals

- Modifiers (`!` and `[]`).
- Nullability defaults.

## 1. Non-Null (!)

By default, **every** field in GraphQL is nullable.
Use `!` to enforce a value.

```graphql
type User {
  id: ID! # Cannot be null
  name: String # Can be null
}
```

## 2. Lists ([])

Arrays of things.

```graphql
type User {
  posts: [Post] # List of Posts. List can be null. Post can be null. (null, [null, Post], ...)
  emails: [String!]! # List cannot be null. Items cannot be null. ([] or ["a"])
}
```

## Key Takeaways

- Be careful with `!`. If a database lookup fails for a required field, the **Entire Object** becomes null (null bubbling).
- Default to Nullable for flexibility, Non-Null for IDs and guarantees.
