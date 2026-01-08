# 13. Using Repository to Access Database

## 🎯 Learning Goal

Replace in-memory array with SQL queries.

## 🧠 Concept

Inject `Repository<Coffee>` into `CoffeesService`.

## 💻 Implementation

```typescript
// coffees.module.ts
imports: [TypeOrmModule.forFeature([Coffee])]

// coffees.service.ts
constructor(
  @InjectRepository(Coffee)
  private readonly coffeeRepository: Repository<Coffee>,
) {}

async findAll() {
  return this.coffeeRepository.find();
}

async create(createCoffeeInput: CreateCoffeeInput) {
  const coffee = this.coffeeRepository.create(createCoffeeInput);
  return this.coffeeRepository.save(coffee);
}
```

## 🧩 Activity / Challenge

1.  Update Service methods.
2.  Create a coffee via Playground.
3.  Check Postgres table.

## 🔑 Key Takeaways

- The stack is complete: Request -> Resolver -> Service -> Repository -> DB.
