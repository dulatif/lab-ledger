# 5. Introduction to Resolvers and Object Types

## 🎯 Learning Goal

Fix the boot error by creating a Resolver.

## 🧠 Concept

**Resolver**: Equivalent to a Controller in REST. It "resolves" a query to data.
**ObjectType**: Defines the shape of the data returned (The Model).

## 💻 Implementation

### 1. The Entity (Object Type)

```typescript
import { ObjectType, Field, Int } from "@nestjs/graphql";

@ObjectType()
export class Coffee {
  @Field((type) => Int)
  id: number;

  @Field()
  name: string;

  @Field()
  brand: string;
}
```

### 2. The Resolver

```typescript
@Resolver((of) => Coffee)
export class CoffeesResolver {
  @Query((returns) => [Coffee])
  async coffees() {
    return [];
  }
}
```

## 🧩 Activity / Challenge

1.  Create `coffees/entities/coffee.entity.ts`.
2.  Create `coffees/coffees.resolver.ts`.
3.  Register Resolver in `CoffeesModule` providers.
4.  App should now start!

## 🔑 Key Takeaways

- `@ObjectType` decorates a class to be a GraphQL Type.
- `@Field` exposes a property. If you omit `@Field`, the property is internal (hidden).
