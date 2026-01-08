# 15. Understanding Durable Providers

## 🎯 Learning Goal

Optimize Request-Scoped providers for multi-tenant applications.

## 🧠 Concept

Problem:

- We have 100 Tenants (Customers).
- We use Request Scope because we need to know "Which Tenant?".
- Result: Controller is recreated 10,000 times a second. CPU 🔥.

Solution: **Durable Providers**.
Instead of 1 instance per **Request**, we have 1 instance per **Tenant ID**.
If 50 requests come from Tenant A, reuse the **same** instance.
If 1 request comes from Tenant B, create a **new** instance.

## 💻 Implementation

```typescript
@Injectable({
  scope: Scope.REQUEST,
  durable: true, // 💎 The Magic Switch
})
export class TenantService {}
```

## 🧩 Activity / Challenge

1.  This feature requires a `ContextIdStrategy` (next lesson).
2.  Without the strategy, `durable: true` does nothing.

## 🔑 Key Takeaways

- **Durable**: "Long-lived" request scope.
- **CachingKey**: We cache the instance based on a key (TenantId) instead of the RequestId.
