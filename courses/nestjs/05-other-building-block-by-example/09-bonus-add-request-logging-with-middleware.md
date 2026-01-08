# 9. Bonus: Add Request Logging with Middleware

## 🎯 Learning Goal

Use Middleware to log every incoming request (Method, URL, Duration).

## 🧠 Concept

Middleware is a function that runs **before** the route handler. It has access to the `request`, `response`, and `next` function.
It's exactly the same as Express/Fastify middleware.
Great for: logging, compression, body parsing.

## 💻 Implementation

### 1. Functional Middleware (Simple)

For simple logging, we don't need a class.

```typescript
// src/common/middleware/logger.middleware.ts
import { Request, Response, NextFunction } from "express";

export function logger(req: Request, res: Response, next: NextFunction) {
  console.log(`Request... ${req.method} ${req.originalUrl}`);
  next(); // ➡️ Don't forget this! Or the app hangs.
}
```

### 2. Apply it

Middleware is applied in the `configure()` method of a Module (usually AppModule).

```typescript
import { MiddlewareConsumer, Module, NestModule } from "@nestjs/common";
import { logger } from "./common/middleware/logger.middleware";

@Module({})
export class AppModule implements NestModule {
  configure(consumer: MiddlewareConsumer) {
    consumer.apply(logger).forRoutes("*"); // 🌍 Apply to all routes
  }
}
```

## 🧩 Activity / Challenge

1.  Create the logger middleware.
2.  Register it in `AppModule`.
3.  Add logic to calculate duration:
    ```typescript
    const start = Date.now();
    res.on("finish", () => {
      const duration = Date.now() - start;
      console.log(`Request took ${duration}ms`);
    });
    next();
    ```

## 🔑 Key Takeaways

- Middleware is the first entry point.
- Must call `next()` to continue the chain.
- Configured in `NestModule.configure()`, not in the `@Module` decorator.
