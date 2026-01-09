# Lesson 5: Aliases & Fragments

## Learning Goals

- Aliases.
- Fragments.
- DRY (Don't Repeat Yourself).

## 1. Aliases

You cannot fetch the same field with different arguments twice... unless you alias it.

```graphql
query CompareUsers {
  firstUser: user(id: "1") {
    name
  }
  secondUser: user(id: "2") {
    name
  }
}
```

Result:

```json
{
  "firstUser": { "name": "Alice" },
  "secondUser": { "name": "Bob" }
}
```

## 2. Fragments

Reusable groups of fields.

```graphql
fragment UserFields on User {
  name
  age
  email
}

query {
  user(id: "1") {
    ...UserFields
  }
}
```

## Key Takeaways

- Use **Aliases** to avoid key collisions.
- Use **Fragments** to share UI component data requirements (e.g., `UserCardFragment`).
