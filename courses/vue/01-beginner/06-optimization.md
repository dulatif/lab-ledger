# Lesson 6: Static & Optimization

## Learning Goals

- `v-once`.
- `v-pre`.

## 1. v-once

Render the element and component **once only**. Subsequent re-renders treat the element as static content.
Useful for displaying data that you know will never change after initial load.

```html
<span v-once>This will never change: {{ msg }}</span>
```

## 2. v-pre

Skip compilation for this element and all its children.
It allows you to show raw mustache tags without Vue parsing them.
It improves compilation speed.

```html
<span v-pre>{{ this will not be compiled }}</span>
```

Displays literally: `{{ this will not be compiled }}`.

## Key Takeaways

- Use `v-once` for static headers or sidebars driven by initial data.
- Use `v-pre` rarely, mostly for documentation or displaying code snippets.
