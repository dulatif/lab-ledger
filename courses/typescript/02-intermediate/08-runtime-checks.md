# 8. Runtime Checks (instanceof and typeof)

## 🎯 Learning Goal

Master standard JS checks for narrowing.

## 🧠 Concept

- `typeof`: Good for primitives (`string`, `number`, `boolean`, `symbol`, `undefined`).
  - _Warning_: `typeof null` is `'object'`. `typeof []` is `'object'`.
- `instanceof`: Good for Classes and Dates. Checks prototype chain.

## 💻 Implementation

### instanceof

```typescript
class Bird {
  fly() {}
}
class Fish {
  swim() {}
}

function move(pet: Bird | Fish) {
  if (pet instanceof Bird) {
    pet.fly();
  } else {
    pet.swim();
  }
}
```

## 🧩 Activity / Challenge

1.  Accept a parameter `date: Date | string`.
2.  Use `instanceof Date` to properly call `.toISOString()`.

## 🔑 Key Takeaways

- These are standard JavaScript operators. TS just leverages them.
