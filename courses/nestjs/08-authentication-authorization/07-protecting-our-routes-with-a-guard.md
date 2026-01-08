# 7. Protecting our routes with a Guard

## 🎯 Learning Goal

Lock down your API so only users with a valid JWT can access it.

## 🧠 Concept

Having a `login` route that returns a token is useless if we don't **check** that token on other routes.
We use **Passport** strategies to handle the heavy lifting of:

1.  Extracting the token (e.g., from `Authorization: Bearer <token>`).
2.  Verifying the signature.
3.  Decoding the payload.

## 💻 Implementation

### 1. The JWT Strategy

```typescript
// jwt.strategy.ts
import { ExtractJwt, Strategy } from "passport-jwt";
import { PassportStrategy } from "@nestjs/passport";
import { Injectable } from "@nestjs/common";

@Injectable()
export class JwtStrategy extends PassportStrategy(Strategy) {
  constructor() {
    super({
      jwtFromRequest: ExtractJwt.fromAuthHeaderAsBearerToken(),
      ignoreExpiration: false,
      secretOrKey: "DO NOT USE THIS VALUE. USE ENV VAR.",
    });
  }

  async validate(payload: any) {
    // This return value is injected into `req.user`
    return { userId: payload.sub, email: payload.email };
  }
}
```

### 2. Registering the Strategy

Add `JwtStrategy` to the `providers` in `AuthModule`.

### 3. The Guard

NestJS provides a built-in guard that uses this strategy.

```typescript
// cats.controller.ts
import { UseGuards } from '@nestjs/common';
import { AuthGuard } from '@nestjs/passport';

@UseGuards(AuthGuard('jwt')) // 🛡️ LOCKED!
@Get()
findAll() { ... }
```

## 🧩 Activity / Challenge

1.  Create `JwtStrategy`.
2.  Add it to `AuthModule` providers.
3.  Add `@UseGuards(AuthGuard('jwt'))` to your `UsersController`.
4.  Try to access `/users` without a token (Expect 401).
5.  Login, get a token, and use it in the Authorization header (Expect 200).

## 🔑 Key Takeaways

- **Strategy**: encapsulated logic for how to validate a credential.
- **Guard**: The gatekeeper that applies the strategy to a route.
- `validate()`: The method called after the token is verified. Its return value becomes `request.user`.
