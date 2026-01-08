# 6. Class Providers

## 🎯 Learning Goal

Deep dive into `useClass` to implement the Strategy Pattern, allowing you to dynamically determine which class controls behavior.

## 🧠 Concept

We previously touched on `useClass` to swap a real service for a mock. But its power goes further.
`useClass` allows you to bind a token (which could be an abstract class) to a concrete implementation.

**Abstract Class as a Token?** 🤯
Yes! Unlike Interfaces, Abstract Classes **do** exist at JavaScript runtime. This means:

1.  You can use them as valid NestJS DI tokens.
2.  You can use them as TypeScript types.
    This gives you Java/C# style polymorphism without needing `@Inject('STRING_TOKEN')`.

## 💻 Implementation

### 1. The Abstract Class (Contract)

First, define the contract.

```typescript
// src/database/data-store.abstract.ts
export abstract class DataStore {
  abstract getAll(): any[];
}
```

### 2. Concrete Implementations

Create different versions of logic.

```typescript
@Injectable()
export class SQLDataStore extends DataStore {
  getAll() {
    return ["SQL Data"];
  }
}

@Injectable()
export class MongoDataStore extends DataStore {
  getAll() {
    return ["Mongo Data"];
  }
}
```

### 3. Registration

Bind the Abstract class to the Concrete one.

```typescript
@Module({
  providers: [
    {
      provide: DataStore, // 👈 Using the Abstract Class as token
      useClass: process.env.USE_MONGO ? MongoDataStore : SQLDataStore,
    },
  ],
})
```

### 4. Usage (No @Inject needed!)

Because `DataStore` is a class, NestJS handles the injection automatically.

```typescript
@Injectable()
export class AppService {
  // NestJS looks up 'DataStore' token and finds 'SQLDataStore' instance
  constructor(private readonly store: DataStore) {}
}
```

## 🧩 Activity / Challenge

1.  Create an abstract class `ConfigServiceAbstract` with a method `getEnv()`.
2.  Create `DevConfigService` (returns 'dev') and `ProdConfigService` (returns 'prod').
3.  In your module, provide `ConfigServiceAbstract` using `useClass: DevConfigService`.
4.  Inject `ConfigServiceAbstract` into a controller and check the output.

## 🔑 Key Takeaways

- **Abstract Classes** can be used as DI tokens because they exist at runtime.
- This is cleaner than Interface + String Token because you don't need `@Inject()`.
- This pattern is excellent for **Polymorphism**: swapping strategies (e.g., FilesystemStorage vs S3Storage) based on environment.
