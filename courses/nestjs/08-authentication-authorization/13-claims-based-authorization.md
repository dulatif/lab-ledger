# 13. Claims-based Authorization

## 🎯 Learning Goal

Move beyond simple Roles to granular Permissions (Claims).

## 🧠 Concept

RBAC is great, but rigid.

- RBAC: "Only Admins can delete."
- Claims: "The user has the `can:delete` permission."

This decouples the _Check_ from the _Job Title_. An "Editor" might be given `can:delete` temporarily without becoming a full "Admin".

## 💻 Implementation

It is structurally identical to RBAC! We just check for **Permissions** instead of Roles.

### 1. Define Permissions

```typescript
export enum Permission {
  CreateCat = "create:cat",
  DeleteCat = "delete:cat",
}
```

### 2. The Strategy

The setup is the same.

1.  `@RequirePermissions(Permission.CreateCat)`
2.  `PermissionsGuard` checks `user.permissions.includes(requiredPermission)`.

## 🧩 Activity / Challenge

1.  Clone your Roles setup but rename everything to Permissions.
2.  Update your User entity to have a list of permissions (or map roles to permissions in the JWT payload).
3.  Protect a route with `@RequirePermissions(Permission.DeleteCat)`.

## 🔑 Key Takeaways

- **Granularity**: Claims are more flexible than Roles.
- **Payload Size**: Be careful not to stuff 500 permissions into the JWT. If the list is huge, check it against the DB/Redis in the Guard instead.
