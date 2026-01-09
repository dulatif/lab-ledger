# Lesson 3: Live Queries & Defer

## Learning Goals

- `@defer`.
- `@stream`.

## 1. @defer

Return critical data immediately, stream the rest later.

```graphql
query {
  user(id: "1") {
    name
    ... @defer {
      friends {
        # Slow field
        name
      }
    }
  }
}
```

## 2. @stream

Stream items in a list as they become available.

```graphql
query {
  room {
    messages @stream(initialCount: 5) {
      body
    }
  }
}
```

## Key Takeaways

- These allow you to improve First Contentful Paint (FCP) dramatically.
- Requires a client that supports Multipart Responses.
