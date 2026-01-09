# Lesson 1: Optimization Techniques

## Learning Goals

- `v-memo`.
- Lazy Loading.
- Bundle Analyzers.

## 1. v-memo

Memoize a sub-tree of the template.
It only re-renders if the dependencies change.

```html
<div v-memo="[a, b]">...</div>
```

If `a` and `b` stay the same, the virtual DOM difference check is skipped entirely.
Useful for giant lists (`v-for`).

## 2. Lazy Loading Components

Don't load the admin dashboard for a regular user.

```typescript
import { defineAsyncComponent } from "vue";

const AsyncComp = defineAsyncComponent(
  () => import("./components/MyComponent.vue")
);
```

**Template**:

```html
<AsyncComp />
```

## 3. Bundle Analysis

Use `rollup-plugin-visualizer` in Vite to see what makes your app huge.
Usually: large libraries (moment.js, lodash) that aren't treeshaken.

## Key Takeaways

- Premature optimization is the root of all evil. Only optimize when it's slow.
- Lazy loading routes is the #1 performance win.
