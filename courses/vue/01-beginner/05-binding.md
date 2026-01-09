# Lesson 5: Attribute Binding

## Learning Goals

- The `v-bind` directive.
- The Shorthand `:`.
- Boolean Attributes.

## 1. Binding to Attributes

You cannot use `{{ }}` inside HTML attributes. You must use `v-bind`.

```html
<!-- Full syntax -->
<div v-bind:id="dynamicId"></div>

<!-- Shorthand (Standard) -->
<div :id="dynamicId"></div>
```

## 2. Boolean Attributes

For attributes like `disabled`, `checked`, `required`.
If the value is truthy, the attribute is present.
If null/undefined/false, the attribute is removed.

```html
<button :disabled="isButtonDisabled">Click Me</button>
```

## 3. Dynamically Binding Multiple Attributes

You can bind an object of attributes.

```typescript
const objectOfAttrs = {
  id: "container",
  class: "wrapper",
  style: "background-color:green",
};
```

```html
<div v-bind="objectOfAttrs"></div>
```

## Key Takeaways

- Always use the `:` shorthand (e.g., `:src`, `:href`, `:class`).
- It connects the Model (Script) to the View (Attributes).
