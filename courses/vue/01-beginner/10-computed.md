# Lesson 10: Computed Properties

## Learning Goals

- `computed()`.
- Caching vs Methods.

## 1. Concepts

Computed properties are for complex logic that depends on reactive data.

```typescript
import { computed, ref } from "vue";

const author = reactive({
  name: "John Doe",
  books: ["Vue 2", "Vue 3 Guide", "Vite Basic"],
});

const publishedBooksMessage = computed(() => {
  return author.books.length > 0 ? "Yes" : "No";
});
```

**Template**:

```html
<span>Has published books: {{ publishedBooksMessage }}</span>
```

## 2. Caching

Why not just use a function? `calculateBooks()`?

- **Computed properties are cached** based on their dependencies. It only re-evaluates when `author.books` changes.
- A **Method** invocation will run _every time_ a re-render happens.

## Key Takeaways

- Use `computed` for performance when doing expensive operations (sorting, filtering libraries).
- Computed properties should be **Read-Only** (side-effect free).
