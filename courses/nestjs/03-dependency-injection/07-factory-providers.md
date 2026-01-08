# 7. Factory Providers

## 🎯 Learning Goal

Learn to use `useFactory` to create providers dynamically, allowing for complex initialization logic and dependencies on other providers.

## 🧠 Concept

Sometimes, creating a provider isn't as simple as `new Service()`.

- Maybe it needs to change based on another service's value?
- Maybe it needs to calculate something before starting?
- Maybe it depends on dynamic configuration?

**Factory Providers** are functions that return the provider instance. These functions can **inject** other providers as arguments! 🏭

## 💻 Implementation

### 1. Basic Syntax

```typescript
{
  provide: 'CRITICAL_VALUE',
  useFactory: () => {
    const isProd = process.env.NODE_ENV === 'production';
    return isProd ? 100 : 1;
  },
}
```

### 2. Injecting Dependencies into Factory

The real power comes from the `inject` array. The order of tokens in `inject` matches the arguments in `useFactory`.

```typescript
// src/connection.provider.ts
import { ConfigService } from "./config.service";

export const connectionProvider = {
  provide: "DATABASE_CONNECTION",

  // 💉 factory receives ConfigService automatically
  useFactory: (configService: ConfigService) => {
    const url = configService.getDatabaseUrl();
    return createConnection(url);
  },

  // 📋 List dependencies here
  inject: [ConfigService],
};
```

### 3. Registering in Module

```typescript
@Module({
  providers: [
    ConfigService,
    connectionProvider, // Using the object exported above
  ],
})
export class DatabaseModule {}
```

## 🧩 Activity / Challenge

1.  Create a provider `{ provide: 'MESSAGE', useValue: 'Hello' }`.
2.  Create a Factory Provider `{ provide: 'GREETING' }`.
3.  Inject `'MESSAGE'` into your factory.
4.  Make the factory return `'Hello World'` (by appending ' World' to the injected message).
5.  Inject `'GREETING'` into a controller and verify the result.

## 🔑 Key Takeaways

- **useFactory** allows dynamic provider creation.
- **inject**: An array of tokens that populate the factory function's arguments.
- This is essential when a provider depends on configuration or other providers to determine its state _before_ it is created.
