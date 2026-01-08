# 6. Function Helper Types

## 🎯 Learning Goal

Extract types from functions.

## 🧠 Concept

Sometimes you impory a library function but not the type of its arguments or return value.

- `Parameters<T>`: Tuple of argument types.
- `ReturnType<T>`: The return type.

## 💻 Implementation

```typescript
function createId(name: string, age: number): string {
  return name + age;
}

type Args = Parameters<typeof createId>; // [string, number]
type Ret = ReturnType<typeof createId>; // string
```

## 🧩 Activity / Challenge

1.  Create a wrapper function that takes `fn` and `...args: Parameters<typeof fn>`.

## 🔑 Key Takeaways

- Useful when writing Higher Order Functions (HOFs) or Middleware.
