# Lesson 6: Composables

## Learning Goals

- Logic Reuse.
- Convention `use...`.

## 1. What is a Composable?

A function that leverages Vue's Composition API to encapsulate and reuse **stateful logic**.
Equivalent to React Hooks.

## 2. Example: useMouse()

`src/composables/useMouse.ts`:

```typescript
import { ref, onMounted, onUnmounted } from "vue";

export function useMouse() {
  const x = ref(0);
  const y = ref(0);

  function update(event) {
    x.value = event.pageX;
    y.value = event.pageY;
  }

  onMounted(() => window.addEventListener("mousemove", update));
  onUnmounted(() => window.removeEventListener("mousemove", update));

  return { x, y };
}
```

## 3. Usage

```typescript
import { useMouse } from "./composables/useMouse";

const { x, y } = useMouse();
```

## Key Takeaways

- **Composables** are the superpower of Vue 3.
- Libraries like **VueUse** provide hundreds of ready-made composables.
