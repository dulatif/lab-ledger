# Lesson 4: Guards & Lazy Loading

## Learning Goals

- Protect routes (AuthGuard).
- Lazy Load routes.

## 1. Functional Guards

Class-based guards are deprecated. Use functions.

```typescript
export const authGuard: CanActivateFn = (route, state) => {
  const authService = inject(AuthService);
  const router = inject(Router);

  if (authService.isLoggedIn()) {
    return true;
  } else {
    return router.parseUrl("/login");
  }
};
```

**Usage**:

```typescript
{ path: 'dashboard', canActivate: [authGuard], ... }
```

## 2. Lazy Loading

Split your code into chunks. Don't load the "Admin" code for a normal user.

```typescript
export const routes: Routes = [
  {
    path: "admin",
    loadChildren: () =>
      import("./admin/admin.routes").then((m) => m.ADMIN_ROUTES),
    // Or for a single component:
    // loadComponent: () => import('./admin/dashboard.component').then(m => m.DashboardComponent)
  },
];
```

## Key Takeaways

- **Lazy Loading** is critical for startup performance.
- **Guards** run before the route is activated.
