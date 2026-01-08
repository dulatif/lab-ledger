# 6. Accessing loC container

## 🎯 Learning Goal

Master the `ModuleRef` API to navigate the dependency tree dynamically.

## 🧠 Concept

We used `moduleRef.get()` before. But `ModuleRef` can do more.
It can check strictness (only get from _this_ module) or look globally.

## 💻 Implementation

### 1. Strict vs Global

```typescript
this.moduleRef.get(Service, { strict: true });
// Only searches providers registered in CURRENT module.
// Returns undefined if provider is imported from another module.

this.moduleRef.get(Service, { strict: false });
// Searches the entire tree (default behavior).
```

### 2. Creating Instances on the Fly

You can create a new instance of a class that wasn't even defined as a provider!

```typescript
const instance = await this.moduleRef.create(SomeTransientClass);
```

## 🧩 Activity / Challenge

1.  Define a generic class `NotificationHandler`.
2.  Don't put it in `providers`.
3.  Use `moduleRef.create(NotificationHandler)` inside a service to create a fresh instance dynamically.
4.  Note: This instance is not managed by the container (it won't be reused unless you manage it).

## 🔑 Key Takeaways

- **IoC Escape Hatch**: Sometimes you need to manually instantiate classes but still want them to receive their dependencies. `moduleRef.create()` does exactly this.
