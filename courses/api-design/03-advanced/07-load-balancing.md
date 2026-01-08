# 7. Load Balancing Strategies

## 🎯 Learning Goal

Distributing traffic across servers.

## 🧠 Concept

- **Round Robin**: A -> B -> C -> A.
- **Least Connections**: Send to server with fewest active users.
- **IP Hash**: User X always goes to Server A (Sticky Sessions).

## 💻 Implementation

Sticky sessions are an anti-pattern for REST (Stateless). Avoid if possible.
Allows you to scale horizontally.

## 🧩 Activity / Challenge

1.  What happens if Server A crashes? Load Balancer detects health check failure and removes it from rotation.

## 🔑 Key Takeaways

- Stateless APIs make Load Balancing trivial.
