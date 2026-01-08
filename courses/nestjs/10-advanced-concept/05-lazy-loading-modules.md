# 5. Lazy-loading Modules

## 🎯 Learning Goal

Optimize startup time by loading modules only when their route is hit.

## 🧠 Concept

By default, Nest loads **Everything** at startup. This is fine for long-running servers.
For **Serverless Functions** (AWS Lambda), startup time (Cold Start) is critical. Use Lazy Loading to only load the code needed for that specific request.

## 💻 Implementation

### 1. The Wrapper

Instead of `imports: [AuthModule]`, we don't import it in `AppModule`.

```typescript
// app.controller.ts
import { LazyModuleLoader } from "@nestjs/core";

@Controller()
export class AppController {
  constructor(private lazyModuleLoader: LazyModuleLoader) {}

  @Get("auth")
  async lazyTrigger() {
    // ⏳ Load module NOW
    const { AuthModule } = await import("./auth/auth.module");
    const moduleRef = await this.lazyModuleLoader.load(() => AuthModule);

    // Get service from the lazily loaded module
    const authService = moduleRef.get(AuthService);
    return authService.login();
  }
}
```

## 🧩 Activity / Challenge

1.  Create a heavy module.
2.  Don't import it.
3.  Use `LazyModuleLoader` in a controller to load it on demand.
4.  Check logs. You should see "AuthModule dependencies initialized" only when you hit the route.

## 🔑 Key Takeaways

- **Trade-off**: Faster boot time VS Slower first request.
- **Use case**: Serverless functions, or massive monoliths where you only need a small slice of features for specific workers.
