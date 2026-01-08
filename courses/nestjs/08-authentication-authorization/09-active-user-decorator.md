# 9. Active User decorator

## 🎯 Learning Goal

Create a cleanly typed custom decorator to extract the current user from the request, replacing `req.user`.

## 🧠 Concept

In your controllers, you often see:

```typescript
@Get('profile')
getProfile(@Request() req) {
  return req.user; // any type 😢
}
```

This is **untyped** and tied to the HTTP platform.
We want:

```typescript
getProfile(@ActiveUser() user: User) { ... }
```

## 💻 Implementation

### 1. The Decorator

```typescript
// active-user.decorator.ts
import { createParamDecorator, ExecutionContext } from "@nestjs/common";

export const ActiveUser = createParamDecorator(
  (data: unknown, ctx: ExecutionContext) => {
    const request = ctx.switchToHttp().getRequest();
    return request.user;
  }
);
```

### 2. Interface (Optional but recommended)

Define an interface for the payload returned by your JWT Strategy.

```typescript
export interface ActiveUserData {
  sub: number;
  email: string;
}
```

### 3. Usage

```typescript
@Get('profile')
getProfile(@ActiveUser() user: ActiveUserData) {
  console.log(user.email);
}
```

## 🧩 Activity / Challenge

1.  Create the `ActiveUser` decorator (or re-use the one from Chapter 6 bonus).
2.  Use it in a controller to log the user's ID.
3.  Refactor any code using `@Request()` to use this new decorator.

## 🔑 Key Takeaways

- **Type Safety**: No more `any`.
- **Abstraction**: Your controller code looks cleaner and focuses on business entities, not framework request objects.
