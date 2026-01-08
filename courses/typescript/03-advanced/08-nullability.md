# 8. Nullability with NonNullable

## 🎯 Learning Goal

Remove null/undefined from a type.

## 🧠 Concept

`NonNullable<T>` excludes `null` and `undefined` from T.

## 💻 Implementation

```typescript
type T0 = NonNullable<string | number | undefined>; // string | number
```

### Checking Keys

```typescript
interface Props {
  name?: string;
}

// Error: Object is possibly undefined
// const s: string = props.name;

// Force it if you know better (or use NonNullable helper)
function process(name: NonNullable<Props["name"]>) {
  // name is string (not undefined)
}
```

## 🧩 Activity / Challenge

1.  Create a type with optional fields.
2.  Create a function that only accepts the required version of that field.

## 🔑 Key Takeaways

- Essential for strict null checks.
