# Lesson 3: Interceptors

## Learning Goals

- Intercept all requests globally.
- Add Auth Tokens headers.

## 1. Functional Interceptors

In modern Angular, interceptors are functions.

```typescript
// auth.interceptor.ts
import { HttpInterceptorFn } from "@angular/common/http";

export const authInterceptor: HttpInterceptorFn = (req, next) => {
  const token = localStorage.getItem("token");

  // Clone the request to add the header
  const authReq = token
    ? req.clone({
        setHeaders: { Authorization: `Bearer ${token}` },
      })
    : req;

  return next(authReq);
};
```

## 2. Registration

Register it in `app.config.ts`.

```typescript
providers: [provideHttpClient(withInterceptors([authInterceptor]))];
```

## 3. Error Handling (Global)

You can also catch errors globally here.

```typescript
return next(req).pipe(
  catchError((err) => {
    if (err.status === 401) {
      // Redirect to login
    }
    return throwError(() => err);
  })
);
```

## Key Takeaways

- **Interceptors** are middleware for your HTTP requests.
- Use them for Logging, Caching, and Authentication.
