# 7. Async Handling with Awaited

## 🎯 Learning Goal

Unwrap Promises recursively.

## 🧠 Concept

`Promise<string>` is annoying if you just want `string`.
`Awaited<T>` models the `await` keyword in types.

## 💻 Implementation

```typescript
type A = Awaited<Promise<string>>; // string
type B = Awaited<Promise<Promise<number>>>; // number (Recursive!)
```

## 🧩 Activity / Challenge

1.  Assume a function `fetchUser(): Promise<User>`.
2.  Use `Awaited<ReturnType<typeof fetchUser>>` to extract the `User` type without importing it directly.

## 🔑 Key Takeaways

- Solves the common "Promise wrapping hell" in types.
