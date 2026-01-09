# Lesson 4: Arguments & Variables

## Learning Goals

- Passing arguments.
- Query Variables ($).

## 1. Arguments

Fields can accept arguments (like function calls).

```graphql
query {
  user(id: "123") {
    name
  }
}
```

## 2. Variables

Hardcoding "123" is bad. Use variables.

1.  Declare the variable in the Query definition.
2.  Pass the variable to the field.

```graphql
query GetUser($userId: ID!) {
  user(id: $userId) {
    name
  }
}
```

**JSON Variables (sent separately)**:

```json
{
  "userId": "123"
}
```

## Key Takeaways

- Never string-concatenate values into a query string. Always use Variables.
- It prevents injection attacks and allows query caching.
