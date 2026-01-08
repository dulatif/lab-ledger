# 5. Hexagonal Architecture in Practice. Part 1

## 🎯 Learning Goal

Define the **Domain Core** and **Ports (Interfaces)** for a Hexagonal application.

## 🧠 Concept

We will build a feature: `RegisterUser`.
The **Core** should not import `@nestjs/common`, TypeORM, or anything external. Pure Logic.

## 💻 Implementation

### 1. Directory Structure

```
src/
  user/
    domain/          # 🧠 Pure Logic
      user.entity.ts
      user.repository.port.ts
    application/     # UseCase orchestrators
      register-user.service.ts
    infrastructure/  # 🔌 Adapters
```

### 2. The Domain Entity

Not a TypeORM entity! Just a class.

```typescript
// src/user/domain/user.entity.ts
export class User {
  constructor(public readonly id: string, public readonly email: string) {}
}
```

### 3. The Port (Interface)

The Domain says: "I need this capabilities."

```typescript
// src/user/domain/user.repository.port.ts
import { User } from "./user.entity";

export const USER_REPO_TOKEN = "UserRepositoryPort";

export interface UserRepositoryPort {
  save(user: User): Promise<void>;
  findByEmail(email: string): Promise<User | null>;
}
```

### 4. The Application Service

The service uses the Port, not a specific class.

```typescript
// src/user/application/register-user.service.ts
@Injectable()
export class RegisterUserService {
  constructor(
    @Inject(USER_REPO_TOKEN) private readonly userRepo: UserRepositoryPort
  ) {}

  async execute(email: string) {
    const existing = await this.userRepo.findByEmail(email);
    if (existing) throw new Error("User exists");

    const user = new User(uuid(), email);
    await this.userRepo.save(user);
  }
}
```

## 🧩 Activity / Challenge

1.  Create the folder structure.
2.  Define the `User` class (Domain).
3.  Define the `UserRepositoryPort` interface.
4.  Implement the `RegisterUserService` logic.
5.  Notice: We haven't installed a database yet! And we don't need to! We can test this logic purely.

## 🔑 Key Takeaways

- **Domain**: Defines the _What_ (Interface).
- **Infrastructure**: Defines the _How_ (Implementation).
- The Application layer orchestrates the flow using dependency injection tokens.
