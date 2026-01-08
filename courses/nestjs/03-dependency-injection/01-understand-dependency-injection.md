# 1. Understand Dependency Injection

## 🎯 Learning Goal

Understand the core concept of **Dependency Injection (DI)** in NestJS and how it promotes loose coupling and testability in your applications.

## 🧠 Concept

Imagine you're building a car 🚗.

- **Without DI**: You (the car class) have to manufacture your own engine inside your factory. If you want to switch from a V6 to a V8 or an Electric engine, you have to modify the car assembly line itself. This is "tight coupling."
- **With DI**: You ask for an "Engine" interface. The factory (NestJS IOC Container) hands you a finished Engine when you are created. You don't care if it's V6, V8, or Electric, as long as it starts and drives. This is **Inversion of Control (IoC)**.

In NestJS, classes (Controllers, Services) declare what they need (dependencies) in their `constructor`, and the framework handles creating and passing those instances to them.

## 💻 Implementation

The most common form of DI in NestJS uses **Constructor Injection**.

### 1. Define a Provider

First, we create a service and decorate it with `@Injectable()`. This marks it as a provider that can be managed by the IoC container.

```typescript
// src/cats/cats.service.ts
import { Injectable } from "@nestjs/common";

@Injectable()
export class CatsService {
  private readonly cats: string[] = ["Garfield"];

  findAll(): string[] {
    return this.cats;
  }
}
```

### 2. Inject the Provider

Next, we ask for this service in a Controller. We use the type `CatsService` as a token.

```typescript
// src/cats/cats.controller.ts
import { Controller, Get } from "@nestjs/common";
import { CatsService } from "./cats.service";

@Controller("cats")
export class CatsController {
  // 💡 NestJS sees 'CatsService' and looks for a matching provider
  constructor(private readonly catsService: CatsService) {}

  @Get()
  findAll(): string[] {
    return this.catsService.findAll();
  }
}
```

### 3. Register the Provider

Finally, we must tell the Module about this provider so NestJS knows it exists.

```typescript
// src/cats/cats.module.ts
import { Module } from "@nestjs/common";
import { CatsController } from "./cats.controller";
import { CatsService } from "./cats.service";

@Module({
  controllers: [CatsController],
  providers: [CatsService], // <--- Registration happens here
})
export class CatsModule {}
```

## 🧩 Activity / Challenge

1.  Create a simple `GreetingService` with a method `sayHello(name: string)`.
2.  Inject it into a `GreetingController`.
3.  Ensure both are registered in your `AppModule`.
4.  Try creating a request to your endpoint and verify it returns the greeting.

> [!TIP]
> Use the `private readonly` shorthand in the constructor. It automatically creates a class property and assigns the argument to it.
>
> ```typescript
> constructor(private readonly service: Service) {}
> // is equivalent to:
> private service: Service;
> constructor(service: Service) { this.service = service; }
> ```

## 🔑 Key Takeaways

- **dependency injection** delegates the responsibility of instantiating dependencies to the NestJS runtime (IoC Container).
- Classes declare their dependencies using **constructor injection**.
- Standard classes must be decorated with `@Injectable()` to be providers.
- Providers must be registered in the `@Module({ providers: [...] })` array.
