# 3. Microservices and API Gateways

## 🎯 Learning Goal

Managing distributed chaos.

## 🧠 Concept

**Microservices**: Splitting Monolith into OrderService, UserService, etc.
**API Gateway**: The single entry point. Handles Auth, Rate Limiting, Routing.
Client -> Gateway -> [Service A, Service B].

## 💻 Implementation

Role of Gateway (Kong, Nginx):

- SSL Termination.
- Request Aggregation (Advanced).
- Protocol Translation (REST -> gRPC).

## 🧩 Activity / Challenge

1.  Accessing User data from Order Service.
    - Direct HTTP call? (Coupling).
    - Eventual consistency via Messaging? (Better).

## 🔑 Key Takeaways

- Gateway pattern hides the complexity of your topology from the client.
