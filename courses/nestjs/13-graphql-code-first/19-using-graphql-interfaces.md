# 19. Using GraphQL Interfaces

## 🎯 Learning Goal

Polymorphism in GraphQL.

## 🧠 Concept

`Drink` Interface. `Coffee` and `Tea` implement it.
Query returns list of `Drink`.

## 💻 Implementation

```typescript
@InterfaceType()
export abstract class Drink {
  @Field()
  name: string;
}

@ObjectType({ implements: Drink })
export class Coffee implements Drink { ... }

@ObjectType({ implements: Drink })
export class Tea implements Drink { ... }
```

Resolver:

```typescript
@Query(returns => [Drink])
async drinks(): Promise<Drink[]> { ... }

@ResolveField()
__resolveType(value) {
  if (value instanceof Coffee) return 'Coffee';
  return 'Tea';
}
```

## 🧩 Activity / Challenge

1.  Query:
    ```graphql
    {
      drinks {
        name
        ... on Coffee {
          brand
        }
      }
    }
    ```

## 🔑 Key Takeaways

- Useful for Heterogeneous collections.
