# Lesson 4: Pagination

## Learning Goals

- Offset vs Cursor.
- Relay Connections.

## 1. Offset Pagination

`skip: 10, limit: 10`.

- **Problem**: If an item is added, pages shift. "Missing" or "Duplicate" items.

## 2. Cursor Pagination (Relay Style)

`first: 10, after: "cursor"`.

- **Cursor**: A pointer to a specific item (usually derived from ID or Time).
- **Result**: Stable pagination.

```graphql
query {
  users(first: 10, after: "abc") {
    edges {
      cursor
      node {
        name
      }
    }
    pageInfo {
      hasNextPage
    }
  }
}
```

## Key Takeaways

- Use Cursor pagination for infinite scrolling or real-time lists.
- The "Relay Connection Specification" is the standard.
