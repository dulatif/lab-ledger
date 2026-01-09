# Lesson 5: Advanced Data Fetching

## Learning Goals

- Beyond `fetch`.
- TanStack Query (Vue Query).

## 1. The Problem with bare fetch

- No caching.
- No deduping (multiple components asking for same data).
- Manual loading/error state management.

## 2. TanStack Query

Wraps promises with powerful state management.

```typescript
import { useQuery } from "@tanstack/vue-query";

const { data, isLoading, error } = useQuery({
  queryKey: ["todos"],
  queryFn: fetchTodos,
});
```

## 3. Features

- **Stale-while-revalidate**: Shows old data while fetching new.
- **Window Focus Refetching**: Auto-updates when user comes back to tab.
- **Deduping**: only 1 network request even if 5 components ask for 'todos'.

## Key Takeaways

- Managing async state is hard. Don't do it manually in `useEffect` / `watchEffect`.
- Use a library.
