# 3. Validation with satisfies operator

## 🎯 Learning Goal

Validate structure without widening types.

## 🧠 Concept

Common issue: Giving a variable a type (e.g., `Record<string, string | number>`) loses specific inference.
The `satisfies` operator (TS 4.9+) checks compatibility **without** changing the inferred type.

## 💻 Implementation

```typescript
type Colors = Record<string, string | { r: number; g: number; b: number }>;

// Old Way (Type Loss)
const palette: Colors = {
  red: "red",
  green: { r: 0, g: 255, b: 0 },
};
// palette.red.toUpperCase(); // Error! TS thinks it MIGHT be an object.

// New Way (Preserves Literal Types)
const palette2 = {
  red: "red",
  green: { r: 0, g: 255, b: 0 },
} satisfies Colors;

palette2.red.toUpperCase(); // OK! TS knows 'red' is a string here.
```

## 🧩 Activity / Challenge

1.  Define a config `Record<string, string | number>`.
2.  Create an object with specific keys using `satisfies`.
3.  Access a string property and use string methods.

## 🔑 Key Takeaways

- Use `satisfies` when you want type safety BUT want to keep the precise type of the value (literal types).
