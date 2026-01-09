# Lesson 10: CSS in Vue

## Learning Goals

- Scoped CSS.
- `v-bind` in CSS.

## 1. Scoped Styles

```html
<style scoped>
  .example {
    color: red;
  }
</style>
```

Only applies to elements in _this_ component.

## 2. CSS Modules

Alternative to scoped.

```html
<style module>
  .red {
    color: red;
  }
</style>
```

Access via `$style.red` in template.

## 3. Dynamic CSS (v-bind)

Bind CSS values to JS state.

```html
<script setup>
  const theme = ref("red");
</script>

<style scoped>
  p {
    color: v-bind(theme);
  }
</style>
```

Vue sets a CSS variable on the element (`--vars-ed7...`) and updates it when `theme` changes.

## Key Takeaways

- Use `scoped` by default.
- `v-bind` in CSS allows powerful dynamic themes.
