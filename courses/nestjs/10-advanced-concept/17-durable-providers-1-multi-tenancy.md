# 17. Durable Providers 1: Multi-tenancy

## 🎯 Learning Goal

Implement the core logic for Multi-tenancy using Durable Providers.

## 🧠 Concept

We need a **Strategy** to tell NestJS: "Use the value of `X-Tenant-Id` as the cache key".

## 💻 Implementation

### 1. The Strategy

```typescript
// aggregate-by-tenant.strategy.ts
import {
  ContextId,
  ContextIdFactory,
  ContextIdStrategy,
  HostComponentInfo,
} from "@nestjs/core";

export class AggregateByTenantContextIdStrategy implements ContextIdStrategy {
  attach(contextId: ContextId, request: Request) {
    const tenantId = request.headers["x-tenant-id"] || "default";

    // 🌳 Create a subtree ID specific to this tenant
    // If we already saw 'acme', return the existing ID.
    // If new, create new.
    let tenantSubTreeId: ContextId;

    if (tenants.has(tenantId)) {
      tenantSubTreeId = tenants.get(tenantId);
    } else {
      tenantSubTreeId = ContextIdFactory.create();
      tenants.set(tenantId, tenantSubTreeId);
    }

    // Return logic... (This is complex API, simplified explanation)
    return {
      resolve: (info: HostComponentInfo) => {
        return info.isTreeDurable ? tenantSubTreeId : contextId;
      },
    };
  }
}
```

### 2. Register it

```typescript
// main.ts
ContextIdFactory.apply(new AggregateByTenantContextIdStrategy());
```

## 🧩 Activity / Challenge

1.  This is widely considered the hardest feature in NestJS.
2.  Don't stress the code details. Understand the flow:
    - Request enters -> Strategy runs -> Extracts TenantID -> Finds existing Subtree -> Returns cached Service.

## 🔑 Key Takeaways

- Allows building massive SaaS platforms on NestJS with native DI support.
