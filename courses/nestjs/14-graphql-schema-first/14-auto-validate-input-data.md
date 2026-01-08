# 14. Auto-validate Input Data

## 🎯 Learning Goal

Validation in Schema First.

## 🧠 Concept

This is tricky.
The generated classes (`coffee.interface.ts`) are **Interfaces**, not Classes.
Decorators like `@MinLength()` only work on Classes.

**Solution**:

1.  Enable `emitTypenameField, outputAs: 'class'` in generator config (Advanced).
2.  OR create a separate DTO class that implements the interface, add decorators, and cast in the controller.
3.  OR manually maintain DTO classes and mapping.

For this course, we will follow the "Manual DTO" approach as it's clearest.
Create `create-coffee.dto.ts` with decorators.
Use `ValidationPipe`.

## 💻 Implementation

Resolver:

```typescript
@Mutation()
async create(@Args('input', ValidationPipe) input: CreateCoffeeDto) { ... }
```

## 🧩 Activity / Challenge

1.  Create a Class DTO.
2.  Accept it in Resolver (NestJS maps Input Object -> Class DTO).
3.  Validate.

## 🔑 Key Takeaways

- Validation is the biggest pain point in Schema First without extra tooling.
