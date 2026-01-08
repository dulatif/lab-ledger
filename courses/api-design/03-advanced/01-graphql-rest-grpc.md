# 1. GraphQL vs REST vs gRPC

## 🎯 Learning Goal

Choosing the right protocol.

## 🧠 Concept

- **REST**: Simple, Cacheable, Public APIs. Over-fetching/Under-fetching issues.
- **GraphQL**: Flexible, Single Endpoint. Complex caching. Good for rich Clients.
- **gRPC**: Binary (Protobuf), High perf, Streaming. Browser support requires proxy. Good for Microservices.

## 💻 Implementation

- Public API -> REST.
- Internal Service-to-Service -> gRPC.
- Complex Mobile App -> GraphQL.

## 🧩 Activity / Challenge

1.  Scenario: Mobile app showing User Profile + Latest 3 Orders + Top 5 Recommendations.
    - REST: 3 calls or custom endpoint.
    - GraphQL: 1 query.

## 🔑 Key Takeaways

- Don't use GraphQL just because it's trendy. It adds complexity.
