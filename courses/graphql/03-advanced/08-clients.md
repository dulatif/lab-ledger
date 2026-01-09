# Lesson 8: Client Landscape

## Learning Goals

- Data Fetching vs State Management.

## 1. Apollo Client

- The "Standard".
- Heavyweight. Includes normalized cache, local state management, optimistic UI.

## 2. Urql

- Modular and lighter.
- Great plugin system (Exchanges).

## 3. Relay

- Facebook's client.
- Opinionated. Requires specific schema structure (Global ID, Connections).
- extremely performant for massive apps.

## Key Takeaways

- Use `graphql-request` for simple scripts (just fetch).
- Use `Apollo` or `Urql` for React Apps.
