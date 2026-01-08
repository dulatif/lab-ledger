# 5. Using Metadata to Build Generic Guards or Interceptors

## 🎯 Learning Goal

Make your guards "smart" and reusable by attaching custom metadata (like Roles) to your routes and reading it inside the Guard.

## 🧠 Concept

A `RolesGuard` is useless if it hardcodes 'admin'. You want to say:

- Route A needs 'admin'.
- Route B needs 'user'.

We can attach data to the route using the `@SetMetadata` decorator and read it in the Guard using the `Reflector` helper.

## 💻 Implementation

### 1. Create a Decorator

`SetMetadata` is low-level. Best practice is to create a custom decorator.

```typescript
// src/common/decorators/roles.decorator.ts
import { SetMetadata } from "@nestjs/common";

export const Roles = (...roles: string[]) => SetMetadata("roles", roles);
```

### 2. Annotate the Controller

```typescript
@Get()
@Roles('admin') // 🏷️ Tagging this route
findAll() {}
```

### 3. Read Metadata in Guard

Inject `Reflector` and use it to get the metadata.

```typescript
// roles.guard.ts
@Injectable()
export class RolesGuard implements CanActivate {
  constructor(private reflector: Reflector) {} // 🔦 Helper tool

  canActivate(context: ExecutionContext): boolean {
    // 1. Get roles from the handler (method)
    const requiredRoles = this.reflector.get<string[]>(
      "roles",
      context.getHandler()
    );

    if (!requiredRoles) {
      return true; // No roles required, proceed
    }

    // 2. Get user from request (assumes AuthGuard ran first)
    const request = context.switchToHttp().getRequest();
    const user = request.user;

    // 3. Match
    return requiredRoles.some((role) => user.roles?.includes(role));
  }
}
```

## 🧩 Activity / Challenge

1.  Create a `@Public()` decorator (metadata key `'isPublic'`).
2.  Update your `ApiKeyGuard` to check for this metadata.
3.  If `isPublic` is true, return `true` immediately (skipping the check).
4.  Decorate one route with `@Public()` and verify you can access it _without_ the API Key.

## 🔑 Key Takeaways

- **Metadata**: Custom data attached to classes/methods.
- **Reflector**: The tool to retrieve that data at runtime.
- This pattern enables **Declarative Permission** checks (`@Roles('admin')`).
