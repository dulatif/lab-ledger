# 12. Using Repository to Access Database

## 🎯 Learning Goal

Connect Service to Postgres.

## 🧠 Concept

Inject Repository.

## 💻 Implementation

```typescript
constructor(
  @InjectRepository(Coffee)
  private readonly coffeeRepository: Repository<Coffee>,
) {}
```

See Chapter 14 for details. It is identical.

## 🧩 Activity / Challenge

1.  Implement `findAll` and `create`.
2.  Test with Playground.

## 🔑 Key Takeaways

- Reuse your knowledge!
