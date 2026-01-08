# 3. What are Microservices?

## 🎯 Learning Goal

Define the characteristics of a Microservice.

## 🧠 Concept

A Microservice is:

1.  **Independently Deployable**: If I deploy the "Orders" service, checking out "Payments" shouldn't happen.
2.  **Loosely Coupled**: Changing internal implementation of "Orders" shouldn't break "Payments".
3.  **Organized around Business Capabilities**: "Billing", "Shipping", not "DbLayer", "UiLayer".
4.  **Owns its Data**: "Orders" allows NO ONE to touch its database tables. You must ask the API.

## 💻 Implementation

In NestJS, a Microservice application typically doesn't listen on HTTP 3000.
It listens on a standardized protocol like TCP, Redis, or NATS.

## 🧩 Activity / Challenge

1.  Think about Amazon.
2.  Ordering, Recommendations, Prime Video.
3.  These are separate teams, separate codebases (or monorepo), separate deployments.

## 🔑 Key Takeaways

- **Autonomy** is the goal.
