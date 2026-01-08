# 7. Type Guards and Narrowing

## 🎯 Learning Goal

Understand how TypeScript refines types based on code flow.

## 🧠 Concept

**Narrowing**: The process of refining a broader type (like `unknown | string`) into a specific type (`string`) based on runtime checks.

## 💻 Implementation

TypeScript inspects your control flow (if/else, switch, throw).

```typescript
function padLeft(padding: number | string, input: string) {
  if (typeof padding === "number") {
    // padding is number here
    return " ".repeat(padding) + input;
  }
  // padding is string here
  return padding + input;
}
```

## 🧩 Activity / Challenge

1.  Write a function receiving `string | string[]`.
2.  If string, return it.
3.  If array, join it.
4.  Observe type hover in VS Code inside the blocks.

## 🔑 Key Takeaways

- You rarely need to cast (`as`). Just write defensive code, and TS understands it.
