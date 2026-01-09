# Lesson 2: Thinking in Graphs

## Learning Goals

- Nodes & Edges.
- Traversing the Graph.

## 1. The Mental Model

Your data is not a list of tables (SQL). It is a graph of objects.

- **Nodes**: Objects (User, Post, Comment).
- **Edges**: Relationships (User _wrote_ Post, Post _has_ Comments).

## 2. Traversing

In REST, you jump from URL to URL.
In GraphQL, you "crawl" or "traverse" the graph via fields.

`User` -> `posts` -> `comments` -> `author` -> `name`.

You can traverse deep into the graph in a single query.

## Key Takeaways

- Stop thinking in "Endpoints". Start thinking in "Types" and "Relationships".
- The "Schema" maps out this graph.
