# 2. Generic Constraints

## 🎯 Learning Goal

Limit what types can be passed to a Generic.

## 🧠 Concept

Currently, `T` can be anything. But what if we need to access a property, like `.length`?
We use `extends` to constrain the generic type.

## 💻 Implementation

```typescript
interface Lengthwise {
  length: number;
}

function loggingIdentity<T extends Lengthwise>(arg: T): T {
  console.log(arg.length); // OK, because T must have length
  return arg;
}

loggingIdentity("hello"); // OK (string has length)
loggingIdentity([1, 2]); // OK (array has length)
// loggingIdentity(10); // Error (number doesn't have length)
```

## 🧩 Activity / Challenge

1.  Create a function `getProperty<T, K>(obj: T, key: K)`.
2.  Constrain `K` to be `keyof T` (using constraints from Intermediate course).
3.  Observe how TS prevents you from asking for non-existent keys.

## 🔑 Key Takeaways

- Constraints make Generics safer and more useful inside the function body.
