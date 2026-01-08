# 8. Implementing the Circuit Breaker pattern

## 🎯 Learning Goal

Prevent cascading failures when an external service is down.

## 🧠 Concept

If Service A calls Service B, and Service B is down:

1.  Service A waits for timeout (e.g., 30s).
2.  Service A runs out of connections/memory.
3.  Service A crashes.

**Circuit Breaker**:

1.  If Service B fails 5 times, **Open the Circuit**.
2.  All subsequent calls to Service B fail **immediately** (Fast Fail) without waiting.
3.  After 1 minute, **Half-Open** (try 1 request).
4.  If success, **Close Circuit** (Resume normal).

## 💻 Implementation

Use `opossum` (a Node.js circuit breaker).

```bash
npm install opossum
```

```typescript
// http.service.ts
import * as CircuitBreaker from "opossum";

@Injectable()
export class MyHttpService {
  private breaker: CircuitBreaker;

  constructor() {
    this.breaker = new CircuitBreaker(this.axiosCall, {
      timeout: 3000,
      errorThresholdPercentage: 50,
      resetTimeout: 30000,
    });
  }

  async makeRequest() {
    return this.breaker.fire();
  }
}
```

## 🧩 Activity / Challenge

1.  Create a service that calls a non-existent URL.
2.  Add a Circuit Breaker.
3.  Spam the endpoint.
4.  Notice how after a few errors, the error changes from "Connection Refused" to "Circuit Open" (Instant error).

## 🔑 Key Takeaways

- **Resilience**: Systems should bend, not break.
- **Fast Fail**: Better to tell the user "System Busy" instantly than let them hang for 30s.
