# 18. Understanding Custom Scalar Types

## 🎯 Learning Goal

Handle formats like JSON, Date, or URL.

## 🧠 Concept

GraphQL has 5 basic scalars.
For Date, we can use `graphql-scalars` or create our own.

## 💻 Implementation

NestJS comes with `GraphQLDate` (deprecated sometimes) or generic scalar support.
Best practice:

```typescript
import { Scalar, CustomScalar } from "@nestjs/graphql";

@Scalar("Date", (returns) => Date)
export class DateScalar implements CustomScalar<number, Date> {
  // parseValue, serialize, parseLiteral
}
```

## 🧩 Activity / Challenge

1.  Add `createdAt: Date` to Coffee.
2.  Register DateScalar in Module.
3.  Query it.

## 🔑 Key Takeaways

- `serialize`: Backend -> Client.
- `parseValue`: Client (Variables) -> Backend.
- `parseLiteral`: Client (Hardcoded string) -> Backend.
