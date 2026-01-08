# 13. Diving into DI sub-trees

## 🎯 Learning Goal

Understand how **REQUEST** scoped providers affect the dependency injection tree performance.

## 🧠 Concept

In a typical Singleton app, the tree is built **once** at startup.
If you inject a `REQUEST` scoped provider (e.g., accessed the `Request` object), then **that provider and everything that depends on it** must be recreated for **every request**.

This creates a "bubble" or "sub-tree" of transient instances.
If your Root Controller depends on a Request Service, then the _Entire Tree_ becomes request-scoped. 📉 Performance drops.

## 💻 Implementation

We don't "implement" sub-trees; we "manage" them.
The goal is to keep the request-scoped sub-tree as **small as possible**.

## 🧩 Activity / Challenge

1.  Make `CatsService` request-scoped (`@Injectable({ scope: Scope.REQUEST })`).
2.  Inject it into `AppController`.
3.  `AppController` is now effectively request-scoped (recreated every time).
4.  Log inside `AppController` constructor. Standard: logs once. Now: logs on every refresh.

## 🔑 Key Takeaways

- **Viral Effect**: Request Scope is viral. It infects parents.
- **Optimization**: Use `ContextIdFactory` to manually create sub-trees only when needed.
