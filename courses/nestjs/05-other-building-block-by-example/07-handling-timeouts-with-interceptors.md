# 7. Handling Timeouts with Interceptors

## 🎯 Learning Goal

Use an Interceptor to automatically cancel requests that take too long to process.

## 🧠 Concept

Sometimes a DB query or external API call hangs. You don't want the client waiting forever.
Since Interceptors use RxJS streams, we can easily pipe the `timeout` operator! If the stream doesn't emit a value within X milliseconds, it throws an error.

## 💻 Implementation

```typescript
import {
  CallHandler,
  ExecutionContext,
  Injectable,
  NestInterceptor,
  RequestTimeoutException,
} from "@nestjs/common";
import { Observable, TimeoutError, throwError } from "rxjs";
import { catchError, timeout } from "rxjs/operators";

@Injectable()
export class TimeoutInterceptor implements NestInterceptor {
  intercept(context: ExecutionContext, next: CallHandler): Observable<any> {
    return next.handle().pipe(
      timeout(3000), // ⏱️ 3 seconds limit
      catchError((err) => {
        if (err instanceof TimeoutError) {
          return throwError(() => new RequestTimeoutException());
        }
        return throwError(() => err);
      })
    );
  }
}
```

## 🧩 Activity / Challenge

1.  Create `TimeoutInterceptor`.
2.  Create a route that uses `await new Promise(r => setTimeout(r, 5000))` to sleep for 5 seconds.
3.  Apply the interceptor (set limit to 3s).
4.  Hit the route.
5.  You should get a `408 Request Timeout` after 3 seconds, instead of waiting the full 5.

## 🔑 Key Takeaways

- RxJS operators are powerful tools for flow control.
- `timeout()` throws an error if the stream is silent for too long.
- `catchError()` allows you to map that generic error to a specific NestJS HTTP exception.
