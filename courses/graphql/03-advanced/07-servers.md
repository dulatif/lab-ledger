# Lesson 7: Server Landscape

## Learning Goals

- Node.js vs Others.
- Code-First vs Schema-First.

## 1. Libraries

- **Apollo Server**: The most popular. Feature-rich but heavy.
- **GraphQL Yoga**: Lightweight, based on standard Web APIs.
- **Mercurius**: Extremely fast (Fastify based).

## 2. Paradigms

- **Schema-First**: Write `.graphql` file, then write resolvers. (Great for design).
- **Code-First** (e.g., Pothos, Nexus): Write TS classes, generate SDL. (Great for type safety).

## Key Takeaways

- If using TypeScript, Code-First is often preferred for 100% type safety.
