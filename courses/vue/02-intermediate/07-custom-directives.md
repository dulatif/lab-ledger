# Lesson 7: Custom Directives

## Learning Goals

- Low-level DOM access.
- The `v-focus` example.

## 1. Definition

Directives are primarily for reusing logic that involves low-level DOM access on plain elements.

```typescript
const vFocus = {
  mounted: (el) => el.focus(),
};
```

**Template**:

```html
<input v-focus />
```

## 2. Lifecycle Hooks

Directives share similar hooks to components:

- `created`
- `mounted` (most common)
- `updated`
- `unmounted`

## 3. Passing Values

```html
<div v-color="'red'"></div>
```

```typescript
const vColor = {
  mounted(el, binding) {
    el.style.color = binding.value;
  },
};
```

## Key Takeaways

- Use directives sparingly.
- Prefer Components for UI reuse.
- Prefer Composables for Logic reuse.
- Directives are for **DOM** reuse.
