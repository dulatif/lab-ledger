# 14. Adding Update and Delete Operations

## 🎯 Learning Goal

Finish CRUD.

## 🧠 Concept

For Update, we need `UpdateCoffeeInput`.
It should act like `PartialType` (all fields optional).
In GraphQL, we use `PartialType` from `@nestjs/graphql`.

## 💻 Implementation

```typescript
@InputType()
export class UpdateCoffeeInput extends PartialType(CreateCoffeeInput) {}

@Mutation(returns => Coffee)
async updateCoffee(
  @Args('id') id: number,
  @Args('updateCoffeeInput') updateCoffeeInput: UpdateCoffeeInput
) {
  // Service logic...
}
```

## 🧩 Activity / Challenge

1.  Implement `update` and `remove` methods in Service.
2.  Expose them in Resolver.
3.  Test via Playground.

## 🔑 Key Takeaways

- `PartialType` utility is a lifesaver for Inputs.
