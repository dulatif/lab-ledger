# 4. Value based Providers

## 🎯 Learning Goal

Learn how to inject static values, simple objects, or configuration constants using `useValue`.

## 🧠 Concept

Sometimes, dependencies aren't classes. Sometimes they are:

- A configuration object (API keys, URLs).
- A specific pre-created testing object.
- A third-party library instance that doesn't use classes.

For these cases, we don't want NestJS to instantiate a class (`useClass`). We want to pass a specific **value** directly. We use the `useValue` syntax.

## 💻 Implementation

### 1. Injecting a Simple Object

Let's mock a service completely using a simple JavaScript object (POJO) instead of a class. This is extremely fast for unit testing.

```typescript
const mockCatsService = {
  findAll: () => ['Test Cat'],
};

@Module({
  providers: [
    {
      provide: CatsService,
      useValue: mockCatsService, // 📦 Inject this specific object
    },
  ],
})
```

### 2. Injecting Configuration Constants

You can also injections strings or configuration objects. Since these don't have a class name to use as a token, we usually use a **String Token** or a **Symbol**.

```typescript
// src/constants.ts
export const CONNECTION_STRING = 'CONNECTION_STRING';

// src/app.module.ts
@Module({
  providers: [
    {
      provide: CONNECTION_STRING, // 🔑 String Token
      useValue: 'mongodb://localhost:27017',
    },
  ],
})
```

### 3. Injecting String Tokens

To inject a provider that uses a string token, you cannot use the type shorthand. You must use the `@Inject()` decorator.

```typescript
import { Inject, Injectable } from "@nestjs/common";
import { CONNECTION_STRING } from "./constants";

@Injectable()
export class CatsRepository {
  constructor(@Inject(CONNECTION_STRING) private readonly connection: string) {}

  logConnection() {
    console.log(this.connection); // Output: mongodb://localhost:27017
  }
}
```

> [!NOTE]
> For simple configuration, NestJS also has a dedicated `@nestjs/config` package, but `useValue` is the foundational concept behind it.

## 🧩 Activity / Challenge

1.  Define a string token `'APP_CONFIG'`.
2.  Register a provider in your module with `useValue`: `{ provide: 'APP_CONFIG', useValue: { port: 3000 } }`.
3.  Inject it into a Controller using `@Inject('APP_CONFIG')`.
4.  Return the config from a GET endpoint.

## 🔑 Key Takeaways

- **useValue**: Tells NestJS to use a specific value/object instance instead of creating a new class instance.
- Useful for **mocking** in tests (replacing heavy services with simple objects).
- Useful for **configuration** (injecting connection strings, API keys).
- When using non-class tokens (like strings), you must use the `@Inject('TOKEN')` decorator in the constructor.
