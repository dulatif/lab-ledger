# Lesson 7: Reactivity Fundamentals

## Learning Goals

- `ref()` vs `reactive()`.
- Composition API State.

## 1. ref()

The standard way to declare reactive state.
It takes an inner value and returns a reactive logic object.

```typescript
import { ref } from "vue";

const count = ref(0); // integer

// Access in Script
console.log(count.value); // .value is required!

// Access in Template
// {{ count }} (No .value needed, automatically unwrapped)
```

## 2. reactive()

Creates a reactive proxy of an object (Deep reactivity).
Unlike `ref`, you access properties directly.

```typescript
import { reactive } from "vue";

const state = reactive({ count: 0 });

state.count++; // Works directly
```

## 3. Which to use?

**Recommendation**: Use `ref()` by default for everything.

- `ref` handles primitives (string, boolean, number) AND objects.
- `reactive` only works on objects and has limitations when destructuring (loses reactivity).

## Key Takeaways

- **Reactivity**: When the state changes, the DOM updates automatically.
- Remember `.value` when using refs in JavaScript.
