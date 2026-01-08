# 3. Debugging Common Errors

## 🎯 Learning Goal

Identify and fix the dreaded `Nest can't resolve dependencies` error and Circular Dependency loops.

## 🧠 Concept

Dependency Injection fails usually happen for 3 reasons:

1.  **Missing Provider**: You requested `CatsService` but `CatsModule` didn't export it, or `AppModule` didn't import `CatsModule`.
2.  **Circular Dependency**: A depends on B, B depends on A. Nest doesn't know who to start first.
3.  **Scope Mismatch**: Injecting a REQUEST-scoped provider into a SINGLETON service (without careful handling).

## 💻 Implementation

### 1. Fixing Circular Dependencies

Use `forwardRef`.

```typescript
// cats.service.ts
constructor(
  @Inject(forwardRef(() => OwnersService))
  private ownersService: OwnersService
) {}

// cats.module.ts
imports: [forwardRef(() => OwnersModule)]
```

### 2. Debugging "Unknown Provider"

Look at the error message carefully!
`Nest can't resolve dependencies of the CatsController (?). Please make sure that the argument CatsService at index [0] is available in the CatsModule context.`

- **Check**: Is `CatsService` in `providers: []` of `CatsModule`?
- **Check**: If using it in `AppModule`, is `CatsModule` imported? And does `CatsModule` **export** `CatsService`?

## 🧩 Activity / Challenge

1.  Intentionally break your app.
2.  Remove `CatsService` from the module exports.
3.  Observe the error.
4.  Create a circular dependency between two services.
5.  Fix it using `forwardRef`.

## 🔑 Key Takeaways

- **Exports**: Providers are private by default. You MUST export them to use them elsewhere.
- **ForwardRef**: The escape hatch for circular imports. Use sparingly; prefer refactoring (creating a third shared service).
