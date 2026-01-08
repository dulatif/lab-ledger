# 6. Add Pointcuts with Interceptors

## 🎯 Learning Goal

Use Interceptors to modify the **Response** sent back to the user (e.g., wrapping all data in a standard object structure).

## 🧠 Concept

Interceptors wrap the execution flow. They can run verify logic _before_ the handler AND _after_ the handler.
This is heavily based on **RxJS Observables**.
The `next.handle()` method returns an Observable which represents the response flow. You can use operators like `map()` to transform the result.

## 💻 Implementation

### Wrap Response Interceptor

Let's make every endpoint return `{ data: ... }` instead of the raw array/object.

```typescript
// src/common/interceptors/wrap-response.interceptor.ts
import {
  CallHandler,
  ExecutionContext,
  Injectable,
  NestInterceptor,
} from "@nestjs/common";
import { Observable } from "rxjs";
import { map } from "rxjs/operators";

@Injectable()
export class WrapResponseInterceptor implements NestInterceptor {
  intercept(context: ExecutionContext, next: CallHandler): Observable<any> {
    // 🟢 Before Handler logic (if needed)

    return next.handle().pipe(
      // 🔴 After Handler logic
      map((data) => ({ data }))
    );
  }
}
```

### Usage

```typescript
@UseInterceptors(WrapResponseInterceptor)
@Get()
findAll() {
  return ['Cat 1', 'Cat 2'];
}
// Response: { "data": ["Cat 1", "Cat 2"] }
```

## 🧩 Activity / Challenge

1.  Create the `WrapResponseInterceptor`.
2.  Apply it globally or to a controller.
3.  Check your API response. It should now have the `data` wrapper.

## 🔑 Key Takeaways

- Interceptors use **RxJS**.
- `next.handle()` executes the route handler.
- `.pipe(map(...))` allows you to mutate the response before it is sent to the client.
