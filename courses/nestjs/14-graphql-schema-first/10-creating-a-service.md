# 10. Creating a Service

## 🎯 Learning Goal

Decouple Resolver from Logic.

## 🧠 Concept

Same as Code First.
Resolvers should just delegate.

## 💻 Implementation

```typescript
@Injectable()
export class CoffeesService {
  // Logic
}

@Resolver("Coffee")
export class CoffeesResolver {
  constructor(private readonly coffeesService: CoffeesService) {}

  @Query("coffees")
  async findAll() {
    return this.coffeesService.findAll();
  }
}
```

## 🧩 Activity / Challenge

1.  Move in-memory array logic to Service.
2.  Update Resolver definition.

## 🔑 Key Takeaways

- The Service layer is identical regardless of REST, Code First, or Schema First.
