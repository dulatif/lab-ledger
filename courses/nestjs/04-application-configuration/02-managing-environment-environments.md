# 2. Managing Environment Environments

## 🎯 Learning Goal

Learn how to load different configuration files based on the environment (e.g., Development vs. Production).

## 🧠 Concept

You usually want different settings for different stages:

- **Development**: Local DB, Debug logging.
- **Production**: Cloud DB, Error logging only.

NestJS allows you to specify strictly which file to load using the `envFilePath` option. You can make this dynamic based on `NODE_ENV`.

## 💻 Implementation

### Dynamic File Path

We can look at `NODE_ENV` (which defaults to 'development' in many setups or can be set manually) to decide which file to read.

```typescript
// src/app.module.ts
import { ConfigModule } from "@nestjs/config";

@Module({
  imports: [
    ConfigModule.forRoot({
      envFilePath:
        process.env.NODE_ENV === "production"
          ? ".env.production"
          : ".env.development",
    }),
  ],
})
export class AppModule {}
```

### Global Access

By default, `ConfigModule` is registered only in `AppModule`. If you want to use it in other feature modules without importing it every time, make it global.

```typescript
ConfigModule.forRoot({
  isGlobal: true, // 🌍 Available everywhere!
});
```

## 🧩 Activity / Challenge

1.  Create two files: `.env.development` (PORT=3000) and `.env.production` (PORT=80).
2.  Update `AppModule` to select the file based on `NODE_ENV`.
3.  Run your app with `NODE_ENV=production npm run start:dev` (on Mac/Linux) or `set NODE_ENV=production && npm run start:dev` (Windows CMD).
4.  Verify which port is active.

## 🔑 Key Takeaways

- `envFilePath`: Allows specifying a custom path (or array of paths) to your env file.
- `isGlobal: true`: Makes the ConfigModule available to all other modules without importing it again.
