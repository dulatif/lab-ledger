# Lesson 5: Enums & Tuples

## Learning Goals

- Tuple.
- Enum.

## 1. Tuples

Fixed-length, fixed-type arrays.
Good for CSV rows, coordinate pairs, or React hooks (`useState`).

```typescript
let coord: [number, number] = [10, 20];
let user: [string, number] = ["Alice", 25]; // Name, Age
```

## 2. Enums

A set of named constants.

```typescript
enum Role {
  Admin, // 0
  User, // 1
  Guest, // 2
}

let myRole: Role = Role.Admin;
```

You can specify values: `enum Role { Admin = "ADMIN", ... }`.

## Key Takeaways

- Tuples enforce position.
- Enums replace "Magic Strings".
