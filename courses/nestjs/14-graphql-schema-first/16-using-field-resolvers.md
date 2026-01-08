# 16. Using Field Resolvers

## 🎯 Learning Goal

Resolve specific fields lazily.

## 🧠 Concept

Decorate a method with `@ResolveField()`.

## 💻 Implementation

```typescript
@Resolver("Coffee") // Must match the Type Name
export class CoffeesResolver {
  @ResolveField()
  async flavors(@Parent() coffee: Coffee) {
    const { id } = coffee;
    return this.flavorsService.findByCoffeeId(id);
  }
}
```

## 🧩 Activity / Challenge

1.  Turn off eager loading in Entity.
2.  Implement `flavors` field resolver.
3.  Query it.

## 🔑 Key Takeaways

- `@ResolveField` works exactly the same in Code First and Schema First.
