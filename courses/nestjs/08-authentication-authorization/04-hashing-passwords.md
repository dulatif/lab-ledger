# 4. Hashing Passwords

## 🎯 Learning Goal

Securely store passwords using **Hashing** and **Salting** with `bcrypt`.

## 🧠 Concept

**NEVER STORE PLAIN TEXT PASSWORDS.** 🛑
If your DB is compromised, hackers will have everyone's passwords.

- **Encryption**: Two-way (can be reversed). Bad for passwords.
- **Hashing**: One-way (cannot be reversed). Good for passwords.
- **Salting**: Adding random data to the hash to prevent "Rainbow Table" attacks (pre-computed lists of hashes).

`bcrypt` handles hashing and salting automatically.

## 💻 Implementation

### 1. The Hashing Provider

We can create a simple utility service.

```typescript
// hashing.service.ts
import { Injectable } from "@nestjs/common";
import * as bcrypt from "bcrypt";

@Injectable()
export class HashingService {
  async hash(password: string): Promise<string> {
    const saltOrRounds = 10;
    return bcrypt.hash(password, saltOrRounds);
  }

  async compare(password: string, hash: string): Promise<boolean> {
    return bcrypt.compare(password, hash);
  }
}
```

### 2. Using it in UsersService

When creating a user, hash before saving.

```typescript
async create(createUserDto: CreateUserDto) {
  const hashedPassword = await this.hashingService.hash(createUserDto.password);

  const newUser = {
      ...createUserDto,
      password: hashedPassword, // 🔒 Safe
  };
  // save newUser to DB...
}
```

## 🧩 Activity / Challenge

1.  Import `HashingService` into your `UsersModule`.
2.  Update your `create` method to hash the password.
3.  Log the output. You should see a long string like `$2b$10$...`.

## 🔑 Key Takeaways

- **Bcrypt**: The industry standard for password hashing.
- **Compare**: You never decrypt the stored password. You hash the _input_ password and compare the two hashes.
