# 9. Logic Checks (Equality and Truthiness)

## 🎯 Learning Goal

Use comparisons for narrowing.

## 🧠 Concept

Equality (`===`, `!==`) and Truthiness (`!`, `&&`) narrow types.

## 💻 Implementation

### Equality

```typescript
function example(x: string | number, y: boolean | number) {
  if (x === y) {
    // x and y must both be numbers here
    x.toFixed();
    y.toFixed();
  }
}
```

### Truthiness

```typescript
function printUsers(users?: string[]) {
  if (users) {
    // users is string[] (undefined removed)
    users.forEach((u) => console.log(u));
  }
}
```

## 🧩 Activity / Challenge

1.  Accept `x: string | null | undefined`.
2.  Use `if (x != null)` (loose equality) to check for both null and undefined.

## 🔑 Key Takeaways

- Use `if (!value)` to quickly check for null/undefined/empty string.
