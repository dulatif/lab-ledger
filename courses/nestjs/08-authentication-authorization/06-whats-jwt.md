# 6. What's JWT?

## 🎯 Learning Goal

Understand the anatomy of a JSON Web Token (JWT) and why we use it for stateless authentication.

## 🧠 Concept

A **JWT** is a long string separated by two dots: `aaaaaa.bbbbbb.cccccc`.

### The 3 Parts

1.  **Header**: Algorithm used (e.g., HS256).
2.  **Payload**: The data (Claims). E.g., `{ "sub": 1, "role": "admin" }`.
3.  **Signature**: Using a **Secret Key**, we sign the Header + Payload.

### Why is it special?

- **Stateless**: The server doesn't need to store "Session ID 123 is logged in" in RAM/DB.
- **Trustworthy**: If the user tries to edit the Payload (change "user" to "admin"), the Signature verification will fail because they don't have the Secret Key.

## 💻 Implementation

To use JWTs in NestJS, we register the `JwtModule`:

```typescript
// auth.module.ts
import { JwtModule } from "@nestjs/jwt";

@Module({
  imports: [
    UsersModule,
    JwtModule.register({
      global: true,
      secret: "DO NOT USE THIS VALUE. USE ENV VAR.", // ⚠️ Secret!
      signOptions: { expiresIn: "60s" },
    }),
  ],
  // ...
})
export class AuthModule {}
```

## 🧩 Activity / Challenge

1.  Go to [jwt.io](https://jwt.io).
2.  Type `{ "role": "admin" }` in the payload.
3.  Observe how the Token string changes.
4.  In your `AuthModule`, configure `JwtModule`.
5.  Call `jwtService.sign(payload)` in your login method (from previous lesson).

## 🔑 Key Takeaways

- **NOT Encrypted**: Does NOT hide data. Anyone can decode base64. **Do not put secrets (passwords) in the payload.**
- **Signed**: Guarantees integrity. Data wasn't tampered with.
- **Secret Management**: The `secret` must be strong and kept in `.env`.
