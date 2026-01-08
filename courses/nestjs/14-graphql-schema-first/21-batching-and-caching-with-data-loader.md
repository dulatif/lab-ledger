# 21. Batching and Caching with Data Loader

## 🎯 Learning Goal

Performance optimization.

## 🧠 Concept

Identical implementation to Code First.
The DataLoader logic lives in a separate module/service and is injected into the Resolver.

## 💻 Implementation

Refer to Lesson 23 of the Code First chapter.

## 🧩 Activity / Challenge

1.  Copy your DataLoader logic.
2.  Apply it to the `flavors` field resolver in `CoffeesResolver`.

## 🔑 Key Takeaways

- Performance patterns (N+1 solutions) are agnostic of how you define the schema.
