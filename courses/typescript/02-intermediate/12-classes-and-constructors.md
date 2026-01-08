# 12. Classes and Constructor Parameters

## 🎯 Learning Goal

Efficiently define classes.

## 🧠 Concept

Classes in TS are basically ES6 classes + Type Annotations + Access Modifiers.

## 💻 Implementation

### Boilerplate Reduction

Instead of declaring fields and setting them in the constructor...
TS offers **Parameter Properties**.

```typescript
class User {
  // Explicitly adds 'public name' and 'private age' to the class
  constructor(public name: string, private age: number) {}
}

const u = new User("Alice", 30);
console.log(u.name);
```

## 🧩 Activity / Challenge

1.  Create a class `Book`.
2.  Use parameter properties to define `title` and `author`.
3.  Instantiate it.

## 🔑 Key Takeaways

- Parameter Properties save a lot of typing. Use them.
