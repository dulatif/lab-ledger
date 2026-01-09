# Lesson 8: Scalars & Objects

## Learning Goals

- SDL (Schema Definition Language).
- Primitive Types.

## 1. The Schema

The contract between Client and Server. Defined in SDL.

```graphql
type User {
  id: ID
  name: String
  isActive: Boolean
}
```

## 2. Scalars

The leaves of the graph.

- `Int`: Signed 32-bit integer.
- `Float`: Signed double-precision floating-point value.
- `String`: UTF-8 character sequence.
- `Boolean`: true or false.
- `ID`: Unique identifier (serialized as String).

## Key Takeaways

- Every field must resolve eventually to a Scalar.
- You can define custom scalars (e.g., `Date`, `JSON`) depending on implementation.
