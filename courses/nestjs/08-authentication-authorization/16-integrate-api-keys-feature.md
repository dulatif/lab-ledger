# 16. Integrate API Keys feature

## 🎯 Learning Goal

Implement a custom Passport Strategy to validate incoming API Keys from the `X-API-KEY` header.

## 🧠 Concept

We will use `passport-headerapikey`.

1.  Extract header.
2.  Find Key in DB.
3.  Validate Hash.
4.  Return scope/user.

## 💻 Implementation

### 1. Install Strategy

```bash
npm install passport-headerapikey
```

### 2. The Strategy

```typescript
// api-key.strategy.ts
import { HeaderAPIKeyStrategy } from "passport-headerapikey";

@Injectable()
export class ApiKeyStrategy extends PassportStrategy(
  HeaderAPIKeyStrategy,
  "api-key"
) {
  constructor(private authService: AuthService) {
    super({ header: "X-API-KEY", prefix: "" }, true, async (apiKey, done) => {
      const isValid = await authService.validateApiKey(apiKey);
      if (!isValid) {
        return done(new UnauthorizedException(), null);
      }
      return done(null, true);
    });
  }
}
```

### 3. The Guard

```typescript
@UseGuards(AuthGuard('api-key'))
@Get('hooks')
webhookHandler() { ... }
```

## 🧩 Activity / Challenge

1.  Implement the `ApiKeyStrategy`.
2.  Mock the `validateApiKey` logic (e.g., check if key === "secret123").
3.  Protect a generic route.
4.  Test with curl: `curl -H "X-API-KEY: secret123" localhost:3000/hooks`.

## 🔑 Key Takeaways

- **Hybrid Auth**: You can have some routes protected by JWT (Users) and others by API Key ( integrations) in the same app.
- **Header Strategy**: Passport is flexible enough to handle any credential source.
