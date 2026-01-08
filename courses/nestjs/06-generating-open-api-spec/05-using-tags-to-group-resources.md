# 5. Using Tags to Group Resources

## 🎯 Learning Goal

Organize your Swagger UI into clean, collapsible sections using Tags.

## 🧠 Concept

If you have 50 endpoints, a flat list is unusable.
**Tags** function like folders. Usually, you tag controllers by their resource name (e.g., "Cats", "Users", "Auth").

## 💻 Implementation

### 1. Tagging a Controller

The `@ApiTags` decorator is almost always placed at the Controller level.

```typescript
import { ApiTags } from "@nestjs/swagger";

@ApiTags("cats") // 🏷️ Group everything in this controller under "cats"
@Controller("cats")
export class CatsController {}
```

### 2. Document Order

You can control the order of tags in `main.ts` using `addTag` on the builder.

```typescript
const config = new DocumentBuilder()
  .addTag("auth", "Authentication endpoints") // First
  .addTag("cats", "Cats inventory") // Second
  .build();
```

## 🧩 Activity / Challenge

1.  Import `@ApiTags` in your controller.
2.  Tag it `'my-resource'`.
3.  Reload Swagger UI.
4.  Notice how your endpoints are now grouped under a big header named "my-resource".

## 🔑 Key Takeaways

- `@ApiTags('name')` groups endpoints.
- Applied at the Controller level (usually).
- Can be applied at Method level for cross-cutting endpoints (rare).
