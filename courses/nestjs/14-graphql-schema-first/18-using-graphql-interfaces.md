# 18. Using GraphQL Interfaces

## 🎯 Learning Goal

Polymorphism via SDL Interfaces.

## 🧠 Concept

SDL:

```graphql
interface Drink {
  name: String!
}

type Coffee implements Drink {
  name: String!
  brand: String!
}

type Tea implements Drink {
  name: String!
}
```

## 💻 Implementation

We need to tell NestJS/Apollo how to resolve the type at runtime.

```typescript
@Resolver("Drink")
export class DrinksResolver {
  @ResolveField("__resolveType")
  resolveType(value) {
    if (value instanceof CoffeeEntity) return "Coffee";
    return "Tea";
  }
}
```

## 🧩 Activity / Challenge

1.  Define Interface in SDL.
2.  Implement `__resolveType`.

## 🔑 Key Takeaways

- Note: In code first we used a decorator method. Here we explicitly resolve `__resolveType`.
