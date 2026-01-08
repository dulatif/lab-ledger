# 15. Namespaces and Augmentation

## 🎯 Learning Goal

Extend existing libraries.

## 🧠 Concept

**Namespaces**: Old way to organize code (internal modules). Mostly replaced by ES Modules, but still used in Declaration Files.

**Module Augmentation**: Adding fields to an existing library.

## 💻 Implementation

### Augmenting Express Request

```typescript
// express.d.ts
import { Request } from "express";

declare module "express" {
  interface Request {
    user?: { id: string }; // Add custom property
  }
}
```

## 🧩 Activity / Challenge

1.  Augment the `String` prototype interface (Global Augmentation).
2.  Add a method `toSpongemockCase()`.
3.  Note: You still need to implement it in JS!

## 🔑 Key Takeaways

- Critical for framework development (adding middleware, plugins).
