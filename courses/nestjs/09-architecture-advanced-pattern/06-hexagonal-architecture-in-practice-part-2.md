# 6. Hexagonal Architecture in Practice. Part 2

## 🎯 Learning Goal

Implement the **Adapters** (Infrastructure) that plug into our Ports.

## 🧠 Concept

Now we need to actually save data. We will create an adapter that implements `UserRepositoryPort`.
This adapter _will_ depend on external tools (like a Map, or TypeORM).

## 💻 Implementation

### 1. In-Memory Adapter (For Testing/Dev)

```typescript
// src/user/infrastructure/in-memory-user.repository.ts
import { UserRepositoryPort } from "../domain/user.repository.port";
import { User } from "../domain/user.entity";

export class InMemoryUserRepository implements UserRepositoryPort {
  private readonly users = new Map<string, User>();

  async save(user: User): Promise<void> {
    this.users.set(user.id, user); // 💾 Save to RAM
  }

  async findByEmail(email: string): Promise<User | null> {
    // 🔎 Search in RAM
    for (const user of this.users.values()) {
      if (user.email === email) return user;
    }
    return null;
  }
}
```

### 2. Wiring it up in the Module

This is where the magic happens. We bind the **Token** to the **Implementation**.

```typescript
// src/user/user.module.ts
@Module({
  providers: [
    RegisterUserService,
    {
      provide: USER_REPO_TOKEN, // 🧠 Domain Token
      useClass: InMemoryUserRepository, // 🔌 Infrastructure Implementation
    },
  ],
})
export class UserModule {}
```

## 🧩 Activity / Challenge

1.  Create the `InMemoryUserRepository`.
2.  Register it in `UserModule`.
3.  Create a Controller (Primary Adapter) that calls `RegisterUserService`.
4.  Test it!

## 🔑 Key Takeaways

- We can swap `useClass: InMemoryUserRepository` with `useClass: TypeOrmUserRepository` later, and **zero lines of code** in the Domain/Application layers will change.
- This is ultimate decouplng.
