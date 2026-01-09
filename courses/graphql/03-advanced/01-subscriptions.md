# Lesson 1: Subscriptions

## Learning Goals

- The `subscription` keyword.
- Pub/Sub.

## 1. Syntax

Subscriptions allow the client to listen for events.
Usually implemented over WebSockets.

```graphql
subscription OnCommentAdded($postId: ID!) {
  commentAdded(postId: $postId) {
    id
    content
    author {
      name
    }
  }
}
```

## 2. Server Implementation (Pub/Sub)

1.  **Mutation**: User adds comment -> Publish event `COMMENT_ADDED` channel.
2.  **Subscription**: Server listens to `COMMENT_ADDED` -> Sends payload to Client.

## Key Takeaways

- Use Subscriptions for small, incremental updates (Chat, Notifications).
- For large data updates, polling is sometimes simpler and more robust.
