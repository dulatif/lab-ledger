# 1. Before we get started

## 🎯 Learning Goal

Understand the "Microservices Tax".

## 🧠 Concept

**Distributed Systems** are significantly harder than Monoliths.
In a Monolith, function calls are reliable. In Microservices, network calls can fail, timeout, or hang.
You trade **Development Complexity** (Monolith spaghetti code) for **Operational Complexity** (Kubernetes, Tracing, Network issues).

## 💻 Implementation

We will be using **NestJS Monorepo** mode.
This allows us to manage multiple applications (`apps/orders`, `apps/payments`) in a single repo, sharing code via `libs/`.

## 🧩 Activity / Challenge

1.  Read Martin Fowler's "Microservices Prerequisites".
2.  If you don't have good automated testing or monitoring, DO NOT use microservices.

## 🔑 Key Takeaways

- Don't start with Microservices. Start with a Modular Monolith.
- Only split when you have a specific reason (Scaling team, Scaling performance of one component, Distinct technology stack).
