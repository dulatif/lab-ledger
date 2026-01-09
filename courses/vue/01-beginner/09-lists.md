# Lesson 9: List Rendering

## Learning Goals

- `v-for` Loop.
- The `key` attribute.

## 1. Basic Loop

Iterate over Arrays or Objects.

```typescript
const items = ref([{ message: "Foo" }, { message: "Bar" }]);
```

```html
<li v-for="item in items">{{ item.message }}</li>
```

## 2. With Index

```html
<li v-for="(item, index) in items">{{ index }} - {{ item.message }}</li>
```

## 3. The Key Attribute

**Mandatory**. Vue needs a unique key to track identity for performance and reordering.

```html
<li v-for="item in items" :key="item.id">...</li>
```

_Do not use `index` as key if the list order can change._

## Key Takeaways

- `v-for` has higher priority than `v-if` if used on the same node (but this is bad practice).
- Always use string or number for `:key`.
