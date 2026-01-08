# 4. Accessing Configuration with ConfigService

## 🎯 Learning Goal

Stop using `process.env` directly! Learn to use `ConfigService` for type-safe and testable configuration access.

## 🧠 Concept

Why not just use `process.env.PORT` everywhere?

1.  **Testing**: It's hard to mock `process.env` in unit tests.
2.  **Type Safety**: `process.env` values are always strings (or undefined). `ConfigService` can return numbers or booleans if validated (by Joi/custom logic).
3.  **Encapsulation**: Your business logic shouldn't know _where_ the config comes from (env vars, remote server, file).

## 💻 Implementation

### 1. Inject ConfigService

It comes from `@nestjs/config`.

```typescript
import { Controller, Get } from "@nestjs/common";
import { ConfigService } from "@nestjs/config";

@Controller()
export class AppController {
  constructor(private configService: ConfigService) {}

  @Get()
  getDatabaseUser() {
    // 🛡️ Accessing value safely
    const user = this.configService.get<string>("DATABASE_USER");

    // 🔢 Getting a number (e.g. from Joi defaults)
    const port = this.configService.get<number>("PORT");

    return { user, port };
  }
}
```

### 2. Inferring Types

The generic `<string>` or `<number>` helps TypeScript know what you expect back, but it doesn't _convert_ the value at runtime (unless Joi did it for you).

## 🧩 Activity / Challenge

1.  Inject `ConfigService` into a Controller.
2.  Create an endpoint that returns the current `PORT` and `NODE_ENV`.
3.  Verify that `PORT` comes back as a number (if you used Joi defaults) or a string.

## 🔑 Key Takeaways

- Avoid `process.env` in your Services/Controllers.
- Inject `ConfigService` instead.
- Use `.get<Type>('KEY')` to retrieve values.
- This makes your code more portable and easier to mock during testing.
