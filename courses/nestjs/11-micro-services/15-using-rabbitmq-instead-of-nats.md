# 15. Using RabbitMQ instead of NATS

## 🎯 Learning Goal

Demonstrate the framework-agnostic nature of NestJS Microservices. I promise it's easy.

## 🧠 Concept

NATS is great (Fire and Forget).
**RabbitMQ** is "Smart Broker, Dumb Consumer". It supports:

- Message Acknowledgements (Guaranteed Delivery).
- Persistent Queues (Messages survive server restart).
- Complex Routing Keys.

## 💻 Implementation

### 1. Run RabbitMQ

```yaml
services:
  rabbitmq:
    image: rabbitmq:3-management
    ports: ["5672:5672", "15672:15672"]
```

### 2. Update Code

```typescript
// main.ts
transport: Transport.RMQ,
options: {
  urls: ['amqp://localhost:5672'],
  queue: 'cats_queue',
},
```

That's it. Logic remains the same.

## 🧩 Activity / Challenge

1.  Switch the `notifications` service to use RMQ.
2.  Note: You can mix and match! Some services use NATS, others RMQ.

## 🔑 Key Takeaways

- **RabbitMQ** is the industry standard for reliable messaging.
- **NATS** is for high-throughput, low-latency.
