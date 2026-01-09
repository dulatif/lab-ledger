# Lesson 4: Provide / Inject

## Learning Goals

- Dependency Injection.
- Avoiding Prop Drilling.

## 1. The Problem

Passing props through 5 layers of components (Grandparent -> Parent -> Child -> GrandChild) just to reach the bottom is tedious ("Prop Drilling").

## 2. The Solution

**Provide** data in the ancestor. **Inject** it in the descendant.

**Ancestor (App.vue)**:

```typescript
import { provide, ref } from "vue";

const theme = ref("dark");
provide("theme", theme);
```

**Descendant (DeepChild.vue)**:

```typescript
import { inject } from "vue";

const theme = inject("theme");
```

## 3. Reactivity

If the provided value is a `ref`, it remains reactive in the injected component.

## Key Takeaways

- Great for global configs, themes, or auth user state.
- Don't overuse it: it makes component data flow less explicit than props.
