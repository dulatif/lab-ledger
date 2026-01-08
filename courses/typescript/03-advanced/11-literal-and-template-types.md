# 11. Literal Types and Template Literal Types

## 🎯 Learning Goal

Manipulate string types.

## 🧠 Concept

You can compose string types using template syntax `${Type}`.

## 💻 Implementation

```typescript
type Color = "red" | "blue";
type Quantity = "light" | "dark";

type Palette = `${Quantity}-${Color}`;
// "light-red" | "light-blue" | "dark-red" | "dark-blue"
```

### String Manipulation Utilities

`Uppercase<T>`, `Capitalize<T>`, etc.

## 🧩 Activity / Challenge

1.  Define `Direction = "top" | "bottom" | "left" | "right"`.
2.  Define `BoxProps` which has keys like `marginTop`, `marginBottom`, etc., generated via Template Literals.

## 🔑 Key Takeaways

- Great for typing CSS classes, prop names, or event names.
