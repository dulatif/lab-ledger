# Lesson 5: Security & Complexity

## Learning Goals

- Query Depth.
- Complexity Cost.

## 1. The Attack

A malicious user can nest fields infinitely:
`author { posts { author { posts ... } } }`
This will crash your server (Recursion).

## 2. Mitigation

1.  **Depth Limit**: Max 5 nested levels.
2.  **Complexity Analysis**: Assign "points" to fields.
    - `scalar`: 1 point.
    - `object`: 10 points.
    - `list(N)`: 10 \* N points.
    - Reject query if Cost > 1000.

## Key Takeaways

- Never expose a public GraphQL API without limits.
- Use libraries like `graphql-depth-limit`.
