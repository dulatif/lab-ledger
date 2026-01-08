# 10. Mapped Types

## 🎯 Learning Goal

Iterate over keys to create new types.

## 🧠 Concept

Syntax: `[P in K]: T`
Often used with `keyof`.

## 💻 Implementation

### Recreating Partial

```typescript
type MyPartial<T> = {
  [P in keyof T]?: T[P]; // ? makes it optional
};
```

### Changing Types

```typescript
type Stringify<T> = {
  [P in keyof T]: string; // Force all values to string
};
```

### Key Remapping (as) - Advanced

```typescript
type Getters<T> = {
  [P in keyof T as `get${Capitalize<string & P>}`]: () => T[P];
};
```

## 🧩 Activity / Challenge

1.  Create a type `Booleanify<T>` that turns all properties into `boolean`.
2.  Use it on a User model to create a "Permissions" object structure.

## 🔑 Key Takeaways

- Extremely powerful for generating derived types (e.g. Validation schemas based on Models).
