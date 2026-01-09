# Lesson 9: State Management (Pinia)

## Learning Goals

- Why Pinia?
- Stores, Actions, Getters.

## 1. Setup

Pinia is the official state management library (Successor to Vuex).
It is modular, type-safe, and incredibly simple.

## 2. Defining a Store

`stores/counter.ts`:

```typescript
import { defineStore } from "pinia";
import { ref, computed } from "vue";

// Setup Store syntax (reads like standard Composition API)
export const useCounterStore = defineStore("counter", () => {
  // State
  const count = ref(0);

  // Getter (Computed)
  const doubleCount = computed(() => count.value * 2);

  // Action (Function)
  function increment() {
    count.value++;
  }

  return { count, doubleCount, increment };
});
```

## 3. Using the Store

Any component can use it.

```typescript
import { useCounterStore } from "@/stores/counter";

const store = useCounterStore();

console.log(store.count);
store.increment();
```

## Key Takeaways

- **Store**: A global object hosting state.
- Stores are auto-imported and shared.
- Pinia supports DevTools (Time Travel).
