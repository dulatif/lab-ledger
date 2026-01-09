# Lesson 8: Caching Strategies

## Learning Goals

- The N+1 Problem.
- DataLoader.
- Client Caching.

## 1. The N+1 Problem

If you query a list of 10 posts, and for each post you fetch the "Author".
1 Query for Posts.
10 Queries for Authors.
Total: 11 Requests to DB. This kills performance.

## 2. DataLoader (Server)

A batching utility. It waits 1 tick, collects all IDs needed (1, 2, 3...), and executes ONE query: `SELECT * FROM users WHERE id IN (1, 2, 3)`.

## 3. Normalized Caching (Client)

Clients like Apollo store data in a flat map by ID: `User:1`, `Post:10`.
If you query the same data twice, it hits the cache, not the network.

## Key Takeaways

- You MUST use DataLoader usually to avoid N+1 issues.
- Client-side caching is one of GraphQL's biggest superpowers.
