# Lesson 1: Composition API vs Options API

## Learning Goals

- The two flavors of Vue.
- Why Composition API?

## 1. Options API (Legacy/Alternative)

Groups code by **option types** (`data`, `methods`, `mounted`).

```typescript
export default {
  data() {
    return { count: 0 };
  },
  methods: {
    increment() {
      this.count++;
    },
  },
};
```

**Pros**: Easy for beginners, constrained structure.
**Cons**: Logic for a single feature is split across data, methods, mounted.

## 2. Composition API (Modern)

Groups code by **Logical Concern**.

```typescript
<script setup>
  import {(ref, onMounted)} from 'vue'; const count = ref(0); function
  increment() {count.value++}
</script>
```

**Pros**: better TypeScript support, logic reuse (composables), better code organization for large components.

## 3. Which one?

This course focuses on **Composition API (`<script setup>`)** as it is the current standard for professional Vue development.

## Key Takeaways

- They can coexist in the same app.
- Composition API unlocks the full power of Vue 3.
