# Lesson 2: WebSockets vs SSE

## Learning Goals

- Full-Duplex vs One-Way.
- Protocols.

## 1. WebSockets

- **Pros**: Full bi-directional. Established standard (`graphql-ws` library).
- **Cons**: Custom protocol, harder to load balance/proxy.

## 2. Server-Sent Events (SSE)

- **Pros**: Simple HTTP. Easy to proxy.
- **Cons**: One-way (Server -> Client only). Client must use POST for queries.

## Key Takeaways

- `graphql-transport-ws` is the modern WebSocket standard.
- SSE is gaining popularity because it works over standard HTTP/2.
