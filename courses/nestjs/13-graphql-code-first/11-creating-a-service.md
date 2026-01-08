# 11. Creating a Service

## 🎯 Learning Goal

Separate Business Logic from Transporter Logic.

## 🧠 Concept

Resolvers (like Controllers) should be thin.
They extract arguments, call a Service, and return the result.
All DB logic goes to the Service.

## 💻 Implementation

Move your dummy array logic to `CoffeesService`.

```typescript
@Injectable()
export class CoffeesService {
  private coffees: Coffee[] = [];

  findAll() {
    return this.coffees;
  }
}
```

Inject into Resolver:

```typescript
constructor(private readonly coffeesService: CoffeesService) {}

@Query(...)
findAll() {
  return this.coffeesService.findAll();
}
```

## 🧩 Activity / Challenge

1.  Refactor your Resolver.
2.  Prove it still works.

## 🔑 Key Takeaways

- This pattern allows you to reuse `CoffeesService` in a REST Controller too! (Hybrid App).
