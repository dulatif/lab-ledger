# 1. Introduction to Generics

## 🎯 Learning Goal

Write reusable code that works with multiple types.

## 🧠 Concept

**Generics** allows you to pass types as parameters to functions or classes, just like function arguments.
Instead of `any`, which loses information, Generics preserve the type relationship.

## 💻 Implementation

```typescript
// Without Generics (Lost Type Info)
function identityBad(arg: any): any {
  return arg;
}

// With Generics (Type Info Preserved)
function identity<T>(arg: T): T {
  return arg;
}

const output = identity<string>("myString"); // Type is string
const num = identity(100); // Type inferred as number
```

### Generic Interfaces

```typescript
interface Box<T> {
  contents: T;
}

const numBox: Box<number> = { contents: 42 };
```

## 🧩 Activity / Challenge

1.  Write a generic `ArrayWrapper` class.
2.  Methods: `add(item: T)`, `get(index: number): T`.

## 🔑 Key Takeaways

- Use standard names like `T`, `U`, `V` for type variables unless context demands specificity (`TUser`).
