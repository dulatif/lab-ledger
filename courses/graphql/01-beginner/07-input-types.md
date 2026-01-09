# Lesson 7: Input Types

## Learning Goals

- `input` keyword.
- Complex arguments.

## 1. The Problem

Passing 10 separate arguments is messy.

```graphql
# Bad
createUser(name: "Bob", age: 20, email: "...", address: "...", ...)
```

## 2. The Solution: Input Objects

Define a structured object just for inputs.

```graphql
input CreateUserInput {
  name: String!
  age: Int
  email: String!
}

mutation CreateUser($input: CreateUserInput!) {
  createUser(input: $input) {
    id
  }
}
```

## Key Takeaways

- Inputs are different from Output Types. You cannot use a `Type` as an argument, only an `Input`.
- Keeps your API clean and extensible.
