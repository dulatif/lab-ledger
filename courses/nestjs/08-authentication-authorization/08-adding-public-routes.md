# 8. Adding Public routes

## 🎯 Learning Goal

Create a mechanism to "Open up" specific routes while keeping the application secure by default.

## 🧠 Concept

Strategy: **"Secure by Default"**.
Instead of adding `@UseGuards` to every single controller manually (which you will forget), we should add it **Globally**.

But wait! If we do that, the `/login` route will also require a token, which creates a catch-22 (chicken and egg problem) 🐔🥚.
We need a way to mark specific routes as **Public**.

## 💻 Implementation

### 1. The `@Public` Decorator

We use NestJS metadata.

```typescript
// public.decorator.ts
import { SetMetadata } from "@nestjs/common";

export const IS_PUBLIC_KEY = "isPublic";
export const Public = () => SetMetadata(IS_PUBLIC_KEY, true);
```

### 2. The Global Guard Logic

We extend the standard guard to check for this metadata.

```typescript
// jwt-auth.guard.ts
@Injectable()
export class JwtAuthGuard extends AuthGuard("jwt") {
  constructor(private reflector: Reflector) {
    super();
  }

  canActivate(context: ExecutionContext) {
    const isPublic = this.reflector.getAllAndOverride<boolean>(IS_PUBLIC_KEY, [
      context.getHandler(),
      context.getClass(),
    ]);

    if (isPublic) {
      return true; // 🔓 Bypass JWT check
    }

    return super.canActivate(context); // 🛡️ Run standard JWT check
  }
}
```

### 3. Usage

```typescript
@Public() // 🔓 Open to everyone
@Post('login')
login() { ... }
```

## 🧩 Activity / Challenge

1.  Bind `JwtAuthGuard` globally in `app.module.ts` providers (using `APP_GUARD`).
2.  Create the `@Public` decorator.
3.  Mark your login route as `@Public`.
4.  Verify that other routes are still protected.

## 🔑 Key Takeaways

- **Secure by Default**: Prevent accidental data leaks by requiring auth everywhere.
- **Reflector**: Helper to access metadata attached to handlers/classes.
- **Overriding**: Contextual metadata (method level) overrides class level.
