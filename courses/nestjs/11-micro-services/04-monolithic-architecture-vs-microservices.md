# 4. Monolithic Architecture vs Microservices

## 🎯 Learning Goal

Compare the trade-offs.

## 🧠 Concept

**Monolith**:

- ✅ Simple deployment (1 container).
- ✅ Simple debugging (1 process).
- ✅ Fast communication (in-memory function calls).
- ❌ Single point of failure (memory leak crashes everything).
- ❌ Hard to scale teams (merge conflicts).

**Microservices**:

- ✅ Fault isolation.
- ✅ Independent scaling (100 replicas of Search, 2 replicas of Checkout).
- ❌ Network latency.
- ❌ Distributed transactions (Hard!).

## 💻 Implementation

Your NestJS Monolith is already "Modular". Transitioning to Microservices is often just:

1.  Take `OrderModule`.
2.  Move it to `apps/orders`.
3.  Replace `OrderService` calls with `ClientProxy.send()`.

## 🧩 Activity / Challenge

1.  Review your current architecture.
2.  Verify: Are your modules loosely coupled? If `OrderModule` imports `UserModule` directly, you can't split them easily.

## 🔑 Key Takeaways

- **Modular Monolith** is usually the sweet spot for 90% of companies.
