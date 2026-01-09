# Lesson 8: Conditional Rendering

## Learning Goals

- `v-if`, `v-else`.
- `v-show`.

## 1. v-if

Real conditional rendering. The element is physically added to or removed from the DOM.

```html
<div v-if="type === 'A'">A</div>
<div v-else-if="type === 'B'">B</div>
<div v-else>Not A or B</div>
```

## 2. v-show

Toggles the CSS `display` property (`none`).
The element is **always** rendered in the DOM, just hidden.

```html
<h1 v-show="isVisible">Hello!</h1>
```

## 3. Comparison

- **v-if**: Higher toggle cost. (Lazily loaded). Use if the condition is unlikely to change often.
- **v-show**: Higher initial render cost, very low toggle cost. Use for toggling elements often (e.g., dropdowns, tabs).

## Key Takeaways

- Use `<template v-if="...">` to toggle multiple elements without adding a wrapper `div`.
