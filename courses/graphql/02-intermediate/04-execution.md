# Lesson 4: Execution Flow

## Learning Goals

- Breadth-first execution.
- Resolution.

## 1. The Tree

GraphQL executes a query by walking the tree breadth-first.

1.  Run Root Resolver (Query).
2.  Wait for it to return a Value (or Promise).
3.  Pass that value as `parent` to the child resolvers.
4.  Repeat.

## 2. Parallelism

Sibling fields (fields at the same level) are executed in **Parallel**.
This is why Queries are fast.

## Key Takeaways

- Understanding execution order helps you optimize database calls.
- Resolvers are "Isolate" functions; they only know about their Parent and Args.
