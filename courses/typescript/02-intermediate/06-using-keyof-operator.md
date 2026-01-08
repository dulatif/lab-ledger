# 6. Using the keyof Operator

## 🎯 Learning Goal

Extract keys from an object type.

## 🧠 Concept

`keyof T` yields a Union of known, public property names of T.

## 💻 Implementation

```typescript
interface Person {
  name: string;
  age: number;
}

type PersonKeys = keyof Person; // "name" | "age"

function getProperty(obj: Person, key: PersonKeys) {
  return obj[key];
}

getProperty(user, "name"); // Valid
// getProperty(user, "email"); // Error!
```

## 🧩 Activity / Challenge

1.  Use `keyof` to write a type-safe `pluck` function that extracts a single field from an object.

## 🔑 Key Takeaways

- `keyof` connects values to types. It's the building block of Mapped Types (Advanced).
