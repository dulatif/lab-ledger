# Lesson 1: Enums

## Learning Goals

- Enumeration types.
- Restricting values.

## 1. Syntax

Enums are scalars that are restricted to a specific set of allowed values.

```graphql
enum UserRole {
  ADMIN
  EDITOR
  GUEST
}

type User {
  role: UserRole!
}
```

## 2. Usage

Use Enums for state, roles, types, or any fixed list of options.
They map to Strings or Integers in your database, but to Types in your code.

## Key Takeaways

- Enums provide documentation and validation out of the box.
- If a client sends "SUPERUSER" (which is not in the Enum), the server rejects it automatically.
