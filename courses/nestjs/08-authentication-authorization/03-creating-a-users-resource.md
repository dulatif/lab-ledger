# 3. Creating a Users resource

## 🎯 Learning Goal

Scaffold the `UsersModule` to handle database operations for our authentication system.

## 🧠 Concept

The Authentication system typically delegates the actual data fetching to a User system.
The `AuthService` shouldn't directly touch the database; it should ask `UsersService` to "find a user by email".

## 💻 Implementation

### 1. Generate Resource

```bash
nest g resource users
```

### 2. The UsersService

We need a method to find a user for login validation.

```typescript
// users.service.ts
@Injectable()
export class UsersService {
  private readonly users = []; // ⚠️ In-memory for demo, replace with DB call

  async findOne(email: string): Promise<User | undefined> {
    return this.users.find((user) => user.email === email);
  }

  async create(createUserDto: CreateUserDto) {
    this.users.push(createUserDto);
    return createUserDto;
  }
}
```

### 3. Exports

Must export `UsersService` so `AuthModule` can use it later!

```typescript
// users.module.ts
@Module({
  providers: [UsersService],
  exports: [UsersService], // 👈 Crucial
})
export class UsersModule {}
```

## 🧩 Activity / Challenge

1.  Generate the `users` resource.
2.  Implement `findOne` and `create` (using an array or your real DB).
3.  Ensure `UsersModule` exports the service.

## 🔑 Key Takeaways

- **Separation of Concerns**: `AuthModule` handles security/tokens. `UsersModule` handles storage/retrieval.
- **Exports**: Creating a shared module allows other parts of the app to safely access user data.
