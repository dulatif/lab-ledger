# 10. Bonus: Create Custom Param Decorators

## 🎯 Learning Goal

Create your own `@User()` decorator to extract the user object from the request, keeping your controllers clean.

## 🧠 Concept

Instead of doing:

```typescript
@Get()
findOne(@Request() req) { // 🤢
  const user = req.user;
}
```

We want:

```typescript
@Get()
findOne(@User() user: UserEntity) { // 😍
}
```

## 💻 Implementation

```typescript
// src/common/decorators/user.decorator.ts
import { createParamDecorator, ExecutionContext } from "@nestjs/common";

export const User = createParamDecorator(
  (data: string, ctx: ExecutionContext) => {
    const request = ctx.switchToHttp().getRequest();
    const user = request.user;

    // Optional: Return specific property if 'data' is passed
    // e.g. @User('email') -> user.email
    return data ? user?.[data] : user;
  }
);
```

## 🧩 Activity / Challenge

1.  Creating the `@User()` decorator.
2.  Before using it, mock the user in a Middleware or Guard: `req.user = { id: 1, email: 'test@test.com' }`.
3.  Use `@User() user` in a controller and return it.
4.  Try using `@User('email') email`.

## 🔑 Key Takeaways

- `createParamDecorator` is the factory for creating your own parameter annotations.
- Greatly improves code readability and reusability.
- Abstracts away the underlying platform (removing direct references to `req` object).
