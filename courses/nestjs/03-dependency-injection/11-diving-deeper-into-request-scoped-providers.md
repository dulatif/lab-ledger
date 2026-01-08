# 11. Diving Deeper Into Request-Scoped Providers

## 🎯 Learning Goal

Learn how to access the Request object inside a service and understand the "Bubble Up" effect of request-scoped providers.

## 🧠 Concept

Why use Request Scope? Usually, it's because you need access to the specific **Request** object (headers, IP, authenticated user) inside a Service, without passing it as an argument to every function.

**The Bubble Up Effect** 🫧
If `CatsService` is Request-Scoped, and `CatsController` injects it, then `CatsController` **automatically becomes Request-Scoped too**.
Why? Because the Controller needs a _unique_ instance of the Service for each request, so the Controller itself must be unique for each request.

This chain reaction can impact performance, so be careful!

## 💻 Implementation

### 1. Injecting the Request Object

You can inject the underlying platform request object (Express/Fastify) using the `REQUEST` token.

```typescript
import { Injectable, Scope, Inject } from "@nestjs/common";
import { REQUEST } from "@nestjs/core";
import { Request } from "express";

@Injectable({ scope: Scope.REQUEST }) // 👈 Must be Request Scoped
export class LoggerService {
  constructor(@Inject(REQUEST) private request: Request) {}

  log(message: string) {
    // Access headers, url, etc.
    console.log(`[${this.request.method}] ${this.request.url}: ${message}`);
  }
}
```

### 2. Usage

Just inject `LoggerService` anywhere. You don't need to pass the request object manually.

```typescript
@Controller("cats")
export class CatsController {
  constructor(private logger: LoggerService) {}

  @Get()
  findAll() {
    this.logger.log("Fetching cats...");
    // Output: [GET] /cats: Fetching cats...
  }
}
```

## 🧩 Activity / Challenge

1.  Create a `HeadersService` that builds a summary of all request headers.
2.  Make it Request Scoped and inject `REQUEST`.
3.  Inject this service into a Controller.
4.  Return the header summary as the response body.
5.  Send requests with different custom headers (e.g. `x-custom: 123`) and verify the output changes.

## 🔑 Key Takeaways

- Use `@Inject(REQUEST)` to access the HTTP Request object.
- This forces the provider to be **Request Scoped**.
- **Bubble Up**: Any provider/controller that depends on a request-scoped provider also becomes request-scoped.
- Use standard Singleton scope whenever possible for maximum performance.
