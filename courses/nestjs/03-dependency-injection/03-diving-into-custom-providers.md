# 3. Diving Into Custom Providers

## 🎯 Learning Goal

Understand that the "standard" registration is just syntactic sugar and learn the manual way to register providers using tokens and classes.

## 🧠 Concept

So far, you've seen the "standard" way to register a provider:
`providers: [CatsService]`

This is actually a **short-hand**! 🍬

Under the hood, NestJS converts that into an object with two properties:

1.  **provide**: The "token" (the name/ID used to look it up).
2.  **useClass**: The actual class to instantiate.

Understanding this opens the door to powerful patterns like swapping implementations (e.g., `MockCatsService` vs `RealCatsService`) without changing code that uses the service.

## 💻 Implementation

### The Standard Way (Shorthand)

```typescript
@Module({
  providers: [CatsService],
})
```

### The Explicit Way (Long-form)

This does exactly the same thing as above:

```typescript
@Module({
  providers: [
    {
      provide: CatsService, // 🔑 The Token (used in constructor injection)
      useClass: CatsService, // 🏭 The Blueprint (what actually gets created)
    },
  ],
})
```

### Swapping Implementation

This is where it gets useful. Let's say we want to use a Mock service for testing or development.

```typescript
// A different class implementation
class MockCatsService {
  findAll() { return ['Mock Cat']; }
}

@Module({
  providers: [
    {
      provide: CatsService, // Consumers still ask for 'CatsService'
      useClass: MockCatsService, // But they get 'MockCatsService' instance!
    },
  ],
})
```

The `CatsController` doesn't need to change at all. It still asks for `CatsService`, but the IoC container hands it an instance of `MockCatsService`.

```typescript
// Controller stays clean!
constructor(private catsService: CatsService) {}
```

## 🧩 Activity / Challenge

1.  Take an existing service in your project.
2.  Go to its Module.
3.  Refactor the `providers` array to use the long-form object syntax `{ provide: X, useClass: X }`.
4.  Verify the app still works. This confirms you understand the underlying mechanism.

## 🔑 Key Takeaways

- Standard provider syntax `[Service]` is sugar for `{ provide: Service, useClass: Service }`.
- **provide**: Defines the token used for injection (DI token).
- **useClass**: Defines the class implementation to be instantiated.
- This pattern allows decoupling the _interface_ (token) from the _implementation_ (class).
