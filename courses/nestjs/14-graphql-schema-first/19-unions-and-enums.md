# 19. Unions and Enums

## 🎯 Learning Goal

Unions and Enums in SDL.

## 🧠 Concept

SDL:

```graphql
enum CoffeeType {
  ARABICA
  ROBUSTA
}

union Result = Coffee | Tea
```

## 💻 Implementation

**Enum**: NestJS automatically generates TS enums if configured.
**Union**: Requires `__resolveType` similar to Interfaces.

```typescript
@Resolver('Result')
export class ResultResolver {
  @ResolveField()
  __resolveType(value) { ... }
}
```

## 🧩 Activity / Challenge

1.  Update SDL.
2.  Handle resolution.

## 🔑 Key Takeaways

- SDL makes the contract very visible.
