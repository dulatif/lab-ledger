# Lesson 3: Directives

## Learning Goals

- `@deprecated`.
- Custom Directives.

## 1. Built-in Directives

- `@deprecated(reason: "Use newField instead")`: Marks a field as deprecated in docs.
- `@include(if: $bool)`: Conditionally include this field.
- `@skip(if: $bool)`: Conditionally skip this field.

```graphql
type User {
  oldField: String @deprecated(reason: "Use newField")
  newField: String
}
```

## 2. Custom Directives

Used by tools or server middleware.
Example: `@auth(role: ADMIN)` to protect a field.

## Key Takeaways

- Directives add metadata or dynamic behavior to your schema.
- Use `@deprecated` liberally instead of deleting fields immediately.
