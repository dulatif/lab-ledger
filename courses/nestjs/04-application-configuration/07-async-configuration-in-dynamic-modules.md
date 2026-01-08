# 7. Asynchronously Configure Dynamic Modules

## 🎯 Learning Goal

Solve the "Chicken and Egg" problem: How do you pass config values (like DB config) to another module (like TypeORM) when the ConfigService itself is still initializing? 🐣

## 🧠 Concept

You often see this pattern:
`TypeOrmModule.forRoot({ ... })`

But you shouldn't hardcode credentials there! You want to read them from `ConfigService`.
But `TypeOrmModule` needs the config _at startup_, and `ConfigService` is also provided _at startup_.

Solution: **Async Configuration**.
Most NestJS ecosystem modules (Mongoose, TypeORM, JWT) provide a `.forRootAsync()` method. This allows you to inject `ConfigService` _into_ the module's configuration factory.

## 💻 Implementation

### The Standard Pattern

This is the pattern you will use 99% of the time for external modules.

```typescript
@Module({
  imports: [
    ConfigModule.forRoot(), // 1. Load Config

    TypeOrmModule.forRootAsync({
      imports: [ConfigModule], // 2. Import ConfigModule here
      inject: [ConfigService], // 3. Inject ConfigService

      // 4. Factory function receives the service
      useFactory: (configService: ConfigService) => ({
        type: "postgres",
        host: configService.get("DATABASE_HOST"),
        port: configService.get<number>("DATABASE_PORT"),
        username: configService.get("DATABASE_USER"),
        password: configService.get("DATABASE_PASSWORD"),
        synchronize: true,
      }),
    }),
  ],
})
export class AppModule {}
```

## 🧩 Activity / Challenge

1.  Imagine a `JwtModule` (from `@nestjs/jwt`).
2.  It has a `registerAsync` method.
3.  Write the configuration block to inject `ConfigService` and set the `secret` from `configService.get('JWT_SECRET')`.

## 🔑 Key Takeaways

- Use `forRootAsync` or `registerAsync` when a module relies on configuration values.
- **imports**: Import `ConfigModule` so the factory can use it.
- **inject**: Instruct NestJS to inject `ConfigService` into `useFactory`.
- **useFactory**: Returns the module options object using the values from `ConfigService`.
