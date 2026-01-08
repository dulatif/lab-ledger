# 4. Explicit vs Implicit Dependencies

## 🎯 Learning Goal

Understand how NestJS tracks dependencies and when to manage them manually.

## 🧠 Concept

**Implicit (Standard)**:
You list dependencies in the Constructor. NestJS analyzes the metadata, finds the providers, and instantiates them.

**Explicit**:
Sometimes you don't know what you need until runtime. Or you want to lazily retrieve a provider to avoid circular dependencies without `forwardRef`.
You use `ModuleRef`.

## 💻 Implementation

### using ModuleRef (Service Locator Pattern)

```typescript
import { ModuleRef } from "@nestjs/core";

@Injectable()
export class CatsService implements OnModuleInit {
  private service: OtherService;

  constructor(private moduleRef: ModuleRef) {}

  onModuleInit() {
    // 🔍 Explicitly ask for the instance
    this.service = this.moduleRef.get(OtherService);
  }
}
```

## 🧩 Activity / Challenge

1.  Inject `ModuleRef` in a service.
2.  Use it to get another provider.
3.  Try `this.moduleRef.get(NonExistentService)`. What happens? (Error).
4.  Try `this.moduleRef.get(RequestScopedService)`. (Error: `get()` only works for Singletons. Use `resolve()` for request-scoped).

## 🔑 Key Takeaways

- **Implicit (Constructor)**: Best for type safety and clarity.
- **Explicit (ModuleRef)**: Good for breaking circular chains or dynamic logic.
- `get()`: Synchronous, safe for Singletons.
- `resolve()`: Asynchronous, creates/retrieves Request-scoped instances.
