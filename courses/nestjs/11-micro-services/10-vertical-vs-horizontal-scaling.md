# 10. Vertical vs Horizontal Scaling

## 🎯 Learning Goal

Understand how to scale Microservices using Docker / Kubernetes concepts.

## 🧠 Concept

**Vertical Scaling (Scale Up)**: Buy a bigger server (8GB RAM -> 16GB RAM).

- Limit: You hit the max hardware available.

**Horizontal Scaling (Scale Out)**: Run more copies of the app.

- 1 Instance -> 5 Instances.
- Limit: Almost infinite.

With **NATS** (or any Broker), horizontal scaling is magic.

- You run 5 copies of the `orders` service.
- They all subscribe to `create_order`.
- NATS automatically creates a **Queue Group**.
- When a message comes, NATS gives it to **one** random instance (Load Balancing).

## 💻 Implementation

In `docker-compose.yml`:

```yaml
services:
  orders:
    deploy:
      replicas: 3
```

## 🧩 Activity / Challenge

1.  Set replicas to 3.
2.  Send 10 requests.
3.  Log "Process ID" in the controller.
4.  You should see different Process IDs handling the requests.

## 🔑 Key Takeaways

- Statelessness is key. If `orders` service stores state in memory, you cannot scale horizontally.
- Use Redis for shared state.
