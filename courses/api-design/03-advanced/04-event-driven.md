# 4. Event-Driven Architecture

## 🎯 Learning Goal

Decoupling services.

## 🧠 Concept

Instead of Service A calling Service B...
Service A emits event `OrderCreated`.
Service B listening to Queue processes it.

## 💻 Implementation

Tools: RabbitMQ, Kafka, AWS SQS.
Benefits:

- Service B can be down, event will wait in queue.
- Service C can also listen to `OrderCreated` without changing Service A.

## 🧩 Activity / Challenge

1.  Fire and Forget.
2.  What happens if the event processing fails? (Dead Letter Queues).

## 🔑 Key Takeaways

- EDA increases reliability and scalability but increases debugging difficulty.
