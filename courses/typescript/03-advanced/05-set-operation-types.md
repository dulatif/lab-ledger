# 5. Set Operation Types (Exclude, Extract)

## 🎯 Learning Goal

Filter Union types.

## 🧠 Concept

- `Exclude<T, U>`: Remove types from T that are assignable to U.
- `Extract<T, U>`: Select types from T that are assignable to U.

## 💻 Implementation

```typescript
type T0 = Exclude<"a" | "b" | "c", "a">; // "b" | "c"
type T1 = Extract<"a" | "b" | "c", "a" | "f">; // "a"

// Practical: Removing null
type NonNullString = Exclude<string | null, null>;
```

## 🧩 Activity / Challenge

1.  Defined `Event = 'click' | 'scroll' | 'mousemove'`.
2.  Derive `MouseEvents` using `Extract`.
3.  Derive `NonScrollEvents` using `Exclude`.

## 🔑 Key Takeaways

- Think of Unions as sets of values. These utilities perform set subtraction/intersection.
