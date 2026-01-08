# 10. Real-Time Data (WebSockets & SSE)

## 🎯 Learning Goal

Pushing data to client.

## 🧠 Concept

**WebSockets**: Bi-directional. Chat apps.
**SSE (Server Sent Events)**: Uni-directional (Server -> Client). News feeds, Stock tickers.

## 💻 Implementation

SSE is simpler (just HTTP long-lived connection).
WebSockets require upgrade handshake and stateful server handling.

## 🧩 Activity / Challenge

1.  SSE works over standard HTTP/2 easily.
2.  WebSockets often need sticky sessions or a pub/sub backend (Redis) to scale across servers.

## 🔑 Key Takeaways

- Use SSE if the client doesn't need to speak back heavily.
