# 10. Composition with Mixins

## 🎯 Learning Goal

Create dynamic classes using **Mixins**.

## 🧠 Concept

TypeScript allows you to create a function that returns a Class.
`(BaseClass) => NewClass`.
This is useful for generating DTOs or Guards dynamically.

NestJS uses this for things like `PartialType(SomeDto)`.

## 💻 Implementation

### Creating a Generic AuthGuard

We want `@UseGuards(RoleGuard('admin'))`? Guards are classes, they can't take arguments in the constructor easily (managed by DI).
BUT, we can create a Mixin that returns a _new class_ pre-configured with 'admin'.

```typescript
function RoleGuard(role: string): Type<CanActivate> {
  class MixinRoleGuard implements CanActivate {
    canActivate(context: ExecutionContext) {
      // Logic using 'role' variable (closure)
      return true;
    }
  }
  return mixin(MixinRoleGuard);
}

@UseGuards(RoleGuard('admin'))
```

## 🧩 Activity / Challenge

1.  Implement the `RoleGuard` mixin.
2.  Notice that `RoleGuard('admin')` returns a Class reference, which `@UseGuards` accepts.
3.  NestJS DI container handles instantiating it.

## 🔑 Key Takeaways

- **Mixins**: Powerful way to create dynamic types.
- **OpenAPI**: heavily used in `@nestjs/swagger` (`PickType`, `OmitType`) to generate DTO variations.
