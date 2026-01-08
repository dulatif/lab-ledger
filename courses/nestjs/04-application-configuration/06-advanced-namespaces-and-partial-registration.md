# 6. Advanced: Namespaces and Partial Registration

## 🎯 Learning Goal

Keep your configuration modular! Learn how to register configuration specifically for a feature module so it doesn't pollute the global config space.

## 🧠 Concept

As your app grows, you'll have config for Auth, Database, Stripe, Redis, Mailer, etc.
Dumping everything into one giant `configService.get('stripe.apiKey')` is messy. 🧹

**Config Namespaces** allow you to:

1.  Define configuration for specific features.
2.  Inject _only_ that configuration into the service that needs it.
3.  Get full TypeScript inference for that specific slice.

## 💻 Implementation

### 1. Define a Namespace

Use `registerAs` from `@nestjs/config`.

```typescript
// src/auth/auth.config.ts
import { registerAs } from "@nestjs/config";

export default registerAs("auth", () => ({
  jwtSecret: process.env.JWT_SECRET,
  expiresIn: "60s",
}));
```

### 2. Register/Load it

You can load it in `AppModule` (global) OR `AuthModule`.

```typescript
// src/auth/auth.module.ts
import authConfig from "./auth.config";

@Module({
  imports: [ConfigModule.forFeature(authConfig)], // 👈 Load just for this feature
})
export class AuthModule {}
```

### 3. Inject it Strongly Typed

Instead of `ConfigService`, we inject the **Namespace** itself!

```typescript
import { Inject, Injectable } from "@nestjs/common";
import { ConfigType } from "@nestjs/config";
import authConfig from "./auth.config";

@Injectable()
export class AuthService {
  constructor(
    @Inject(authConfig.KEY) // 🔑 Inject using the namespace key
    private config: ConfigType<typeof authConfig> // 🛡️ Full Type Safety!
  ) {}

  validate() {
    // We can access properties directly with types!
    console.log(this.config.jwtSecret);
  }
}
```

## 🧩 Activity / Challenge

1.  Create `database.config.ts` using `registerAs('database', ...)`.
2.  Use `ConfigModule.forFeature` in a `DatabaseModule`.
3.  Inject the config into a service using `@Inject(databaseConfig.KEY)`.
4.  Observe how VS Code auto-completes properties like `this.config.host`.

## 🔑 Key Takeaways

- **registerAs**: Creates a namespaced config factory.
- **ConfigModule.forFeature**: Loads the config namespace.
- **ConfigType<typeof X>**: Provides automatic type safety for the injected config object.
- **@Inject(config.KEY)**: The token used to inject the namespace.
