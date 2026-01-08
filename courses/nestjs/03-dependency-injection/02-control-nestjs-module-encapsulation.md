# 2. Control NestJS Module Encapsulation

## 🎯 Learning Goal

Master how to organize your code into Modules and control which providers are private and which are public (**encapsulation**).

## 🧠 Concept

In NestJS, **Modules are singletons by default**, but they are also strict boundaries for your providers.

Think of a Module as a **capsule** or a private room 🔒.

- Anything inside `providers: []` is private to that module.
- Other modules **cannot** access these private providers unless you explicitly `export` them.
- To use a provider from another module, you must `import` that module.

This encapsulation prevents spaghetti code where everything can access everything else. It forces you to define clear Public APIs for your feature modules.

## 💻 Implementation

### 1. Creating a Shared Module

Let's say we have a `DatabaseModule` that manages database connections. We want to share `DatabaseService` with the rest of the app.

```typescript
// src/database/database.module.ts
import { Module } from "@nestjs/common";
import { DatabaseService } from "./database.service";

@Module({
  providers: [DatabaseService],
  exports: [DatabaseService], // 🔓 Making it public!
})
export class DatabaseModule {}
```

### 2. Importing the Module

Now, if `UsersModule` needs to use the DB, it imports the _Module_, not the service directly.

```typescript
// src/users/users.module.ts
import { Module } from "@nestjs/common";
import { UsersController } from "./users.controller";
import { UsersService } from "./users.service";
import { DatabaseModule } from "../database/database.module";

@Module({
  imports: [DatabaseModule], // 🔌 Plug in the module
  controllers: [UsersController],
  providers: [UsersService],
})
export class UsersModule {}
```

### 3. Using the Service

Now `UsersService` can inject `DatabaseService`.

```typescript
// src/users/users.service.ts
import { Injectable } from "@nestjs/common";
import { DatabaseService } from "../database/database.service";

@Injectable()
export class UsersService {
  constructor(private readonly dbService: DatabaseService) {}
}
```

> [!WARNING]
> If you try to inject `DatabaseService` without importing `DatabaseModule` (and exporting the service), NestJS will throw an error:
> `Nest can't resolve dependencies of the UsersService...`

## 🧩 Activity / Challenge

1.  Create `MathModule` with a `MathService` (add, subtract methods).
2.  **Don't** export `MathService` at first.
3.  Try to import `MathModule` into `AppModule` and use `MathService`. Observe the error.
4.  Fix it by adding `exports: [MathService]` to `MathModule`.

## 🔑 Key Takeaways

- Modules encapsulate providers. By default, providers are **private**.
- Use the `exports` array to make a provider public (visible to other modules).
- Use the `imports` array to consume the public providers of another module.
- You import **Modules**, not Services directly in the `imports` array.
