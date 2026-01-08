# 1. Before we get started

## 🎯 Learning Goal

Understand that this chapter is for **Power Users**. These features are not for daily use but are life-savers when you hit the complexity wall.

## 🧠 Concept

NestJS hides a lot of complexity (the IoC container, module wiring, instance creation).
Sometimes, standard features aren't enough.

- You might need to optimize boot time (Lazy Loading).
- You might need resilience (Circuit Breaker).
- You might need extreme isolation (Multi-tenancy).

## 💻 Implementation

We are going deep into the `NestFactory` and `ModuleRef` APIs.
Warning: **With great power comes great responsibility.** 🕷️
Don't use `ModuleRef` just because you can. Use standard Dependency Injection whenever possible.

## 🧩 Activity / Challenge

Review your knowledge of:

1.  Provider Scopes (Singleton vs Request).
2.  Dynamic Modules (`registerAsync`).
    We will build on these concepts.

## 🔑 Key Takeaways

- The deeper you go, the more you understand how NestJS works "under the hood".
