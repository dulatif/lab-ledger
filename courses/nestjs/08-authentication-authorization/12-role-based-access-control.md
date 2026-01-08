# 12. Role-based Access Control

## 🎯 Learning Goal

Implement the classic "Admin vs User" permission system using a custom `@Roles` decorator and a Guard.

## 🧠 Concept

**RBAC** is the most common form of Authorization.

- **Role**: A label assigned to a user (e.g., `Admin`, `Editor`, `User`).
- **Check**: "Does the user have the role required to access this resource?"

## 💻 Implementation

### 1. The `@Roles` Decorator

```typescript
// roles.decorator.ts
import { SetMetadata } from "@nestjs/common";
import { Role } from "./role.enum"; // Assume enum Role { ADMIN = 'admin', USER = 'user' }

export const ROLES_KEY = "roles";
export const Roles = (...roles: Role[]) => SetMetadata(ROLES_KEY, roles);
```

### 2. The Roles Guard

The Guard needs to:

1.  Read the **required roles** from the handler metadata.
2.  Read the **user's role** from `request.user` (populated by JWT Strategy).
3.  Compare them.

```typescript
// roles.guard.ts
@Injectable()
export class RolesGuard implements CanActivate {
  constructor(private reflector: Reflector) {}

  canActivate(context: ExecutionContext): boolean {
    const requiredRoles = this.reflector.getAllAndOverride<Role[]>(ROLES_KEY, [
      context.getHandler(),
      context.getClass(),
    ]);
    if (!requiredRoles) {
      return true; // No roles required
    }
    const { user } = context.switchToHttp().getRequest();
    // Check if user has ANY of the required roles
    return requiredRoles.some((role) => user.roles?.includes(role));
  }
}
```

### 3. Usage

```typescript
@UseGuards(JwtAuthGuard, RolesGuard) // Run JWT first to get user, then Roles
@Roles(Role.ADMIN)
@Delete(':id')
remove(@Param('id') id: string) { ... }
```

## 🧩 Activity / Challenge

1.  Define a `Role` enum.
2.  Add a `roles` property to your JWT payload and User entity.
3.  Implement the Guard.
4.  Try to delete a resource as a normal user (Forbidden).
5.  Try to delete as an admin (Success).

## 🔑 Key Takeaways

- **Reflector**: Essential for accessing metadata set by decorators.
- **Composition**: Guards define the rules, Decorators define the constraints.
- **Order Matters**: `JwtAuthGuard` must run _before_ `RolesGuard` so that `request.user` exists.
