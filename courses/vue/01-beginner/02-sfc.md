# Lesson 2: Single File Components (SFC)

## Learning Goals

- The `.vue` file format.
- Script, Template, Style blocks.

## 1. The Concept

Vue defines a component in a single file with 3 distinct blocks.
This allows high cohesion (template, logic, and style live together).

## 2. Anatomy

```html
<!-- logic -->
<script setup>
  import { ref } from "vue";
  const count = ref(0);
</script>

<!-- view -->
<template>
  <button @click="count++">Count is: {{ count }}</button>
</template>

<!-- style -->
<style scoped>
  button {
    font-weight: bold;
  }
</style>
```

## 3. Breakdown

- **`<script setup>`**: compile-time syntactic sugar. Less boilerplate than the standard `<script>`. Variables declared here are automatically available in the template.
- **`<template>`**: The HTML structure.
- **`<style scoped>`**: Styles here **only** apply to this component. Vue adds unique data attributes to elements (e.g., `data-v-f3f3eg9`) to enforce scoping.

## Key Takeaways

- SFCs are compiled into standard JavaScript and CSS by Vite/Vue Loader.
- Always use `scoped` styles to prevent side effects.
