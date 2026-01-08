# 4. Protect Routes with Guards

## 🎯 Learning Goal

Build a basic security Guard to allow or deny requests based on specific criteria (like an API key).

## 🧠 Concept

Guards have a single responsibility: determine whether a request will be handled by the route handler or not.
They implement `CanActivate`.

- Return `true`: Request proceeds. ✅
- Return `false`: Request denied (403 Forbidden). ⛔

This is much cleaner than checking `if (!user) return error` in every single controller method.

## 💻 Implementation

### 1. Create the Guard

Let's make a guard that checks for a secret API key header.

```typescript
// src/common/guards/api-key.guard.ts
import { Injectable, CanActivate, ExecutionContext } from "@nestjs/common";
import { Request } from "express";

@Injectable()
export class ApiKeyGuard implements CanActivate {
  canActivate(context: ExecutionContext): boolean {
    const request = context.switchToHttp().getRequest<Request>();
    const authHeader = request.header("Authorization");

    // 🛡️ Simple logic: Check exact match
    return authHeader === "my-secret-key";
  }
}
```

### 2. Apply the Guard

```typescript
@Controller("cats")
@UseGuards(ApiKeyGuard) // 🔒 Locked down!
export class CatsController {}
```

## 🧩 Activity / Challenge

1.  Create `ApiKeyGuard`.
2.  Bind it to one of your controllers.
3.  Send a request _without_ the header. Expect 403 Forbidden.
4.  Send a request with header `Authorization: my-secret-key`. Expect 200 OK.

## 🔑 Key Takeaways

- Guards must implement `MediaType`. `CanActivate`.
- `ExecutionContext` gives access to the request (similar to `ArgumentsHost` but with more metadata).
- Guards are executed **after** middleware but **before** interceptors and pipes.
