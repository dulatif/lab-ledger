# 1. Interfaces and Interface Declarations

## 🎯 Learning Goal

Understand how to define contracts for objects using Interfaces.

## 🧠 Concept

An **Interface** is a way to define strict contracts within your code and outside of your code (with third-party libraries).
Unlike Classes, Interfaces exist _only_ at compile time. They are completely removed from the JavaScript output.

## 💻 Implementation

### Basic Interface

```typescript
interface User {
  id: number;
  username: string;
  email?: string; // Optional property
  readonly createdAt: Date; // Cannot be changed after creation
}

const user: User = {
  id: 1,
  username: "alice",
  createdAt: new Date(),
};
```

### Declaration Merging

One unique feature of Interfaces is that you can declare them multiple times, and TypeScript will merge them.

```typescript
interface Box {
  height: number;
}
interface Box {
  width: number;
}
// Box now has both height and width
const box: Box = { height: 10, width: 20 };
```

## 🧩 Activity / Challenge

1.  Define an interface `Product` with `name`, `price`, and an optional `description`.
2.  Create a strict object satisfying that interface.
3.  Try to modify a `readonly` property and observe the error.

## 🔑 Key Takeaways

- Use Interfaces for describing data shapes (DTOs, configurations).
- Interfaces are extendable via merging (useful for plugin systems).
