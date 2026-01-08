# 10. Control Providers Scope

## 🎯 Learning Goal

Understand the lifecycle of providers: Singleton (default), Transient, and Request-Scoped, and when to use each.

## 🧠 Concept

By default, almost everything in NestJS is a **Singleton**.

- **Singleton**: 1 instance is created at startup and shared across the entire app. (Efficient! ⚡)

However, sometimes you need different behaviors:

- **Transient**: A new instance is created _every_ time it is injected into a new component.
- **Request-Scoped**: A new instance is created for _every incoming HTTP request_.

## 💻 Implementation

### 1. Default (Singleton)

You don't do anything. This is the default.

```typescript
@Injectable()
export class CatsService {}
```

### 2. Transient Scope

Use the `scope` property.

```typescript
import { Injectable, Scope } from "@nestjs/common";

@Injectable({ scope: Scope.TRANSIENT })
export class CatsService {
  constructor() {
    console.log("CatsService created!"); // Will log multiple times
  }
}
```

### 3. Request Scope

Use `Scope.REQUEST`.

```typescript
@Injectable({ scope: Scope.REQUEST })
export class CatsService {
  // Created fresh for EVERY implementation request
}
```

## 🧩 Activity / Challenge

1.  Create `CounterService` with a `count` property starting at 0 and an `increment()` method.
2.  Set it to `Scope.TRANSIENT`.
3.  Inject it into `CatController` AND `DogController`.
4.  Hit endpoints on both. Notice that the count in one doesn't affect the other? They have separate instances!
5.  Change to Singleton (default) and see how they share the same count state.

## 🔑 Key Takeaways

- **Singleton (Default)**: Best for performance. Cache, DB connections, Logic.
- **Transient**: Lightweight, unique instance per consumer.
- **Request**: **expensive**. Creates instance per request. Use only when necessary (e.g., tailored logging, multi-tenancy).
