# 9. Manipulating Data with Mutations

## 🎯 Learning Goal

Modify server-side data (POST/PUT/DELETE equiv).

## 🧠 Concept

**Query**: Read-only (equivalent to GET). Can happen in parallel.
**Mutation**: Writes. Must happen in series.

## 💻 Implementation

### 1. The DTO (InputType)

We don't use `@ObjectType` for inputs. We use `@InputType`.

```typescript
@InputType()
export class CreateCoffeeInput {
  @Field()
  name: string;

  @Field()
  brand: string;

  @Field((type) => [String])
  flavors: string[];
}
```

### 2. The Resolver

```typescript
@Mutation(returns => Coffee)
async createCoffee(@Args('createCoffeeInput') createCoffeeInput: CreateCoffeeInput) {
  return {}; // Logic to save
}
```

## 🧩 Activity / Challenge

1.  Create `CreateCoffeeInput`.
2.  Implement `createCoffee` mutation.
3.  Run it in Playground:
    ```graphql
    mutation {
      createCoffee(
        createCoffeeInput: { name: "Latte", brand: "Starbucks", flavors: [] }
      ) {
        id
      }
    }
    ```

## 🔑 Key Takeaways

- `@InputType` is for arguments. `@ObjectType` is for return values.
