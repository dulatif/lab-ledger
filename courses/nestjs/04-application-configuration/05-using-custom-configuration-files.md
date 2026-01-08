# 5. Using Custom Configuration Files

## 🎯 Learning Goal

Learn how to define configuration using structured TypeScript files instead of flat `.env` strings, and how to verify which one "wins" when they conflict.

## 🧠 Concept

`.env` files are flat and always strings 📄.
Sometimes you want more structure (nested objects) or calculated values.
NestJS allows you to load **Custom Configuration Files**. These are factory functions that return a config object.

**Hierarchy**:

1.  Environment Variables (System Override) - 🏆 Strongest
2.  `.env` file
3.  Custom Config File

## 💻 Implementation

### 1. Create a Config Factory

Create a file, e.g., `src/config/configuration.ts`.

```typescript
// src/config/configuration.ts
export default () => ({
  port: parseInt(process.env.PORT, 10) || 3000,
  database: {
    host: process.env.DATABASE_HOST,
    port: parseInt(process.env.DATABASE_PORT, 10) || 5432,
  },
});
```

### 2. Load it in AppModule

Pass it to the `load` array.

```typescript
import configuration from './config/configuration';

@Module({
  imports: [
    ConfigModule.forRoot({
      load: [configuration], // 👈 Load the factory
    }),
  ],
})
```

### 3. Usage

Now you can access nested properties using dot notation!

```typescript
// Get the whole database object
const dbConfig = this.configService.get("database");

// Get nested property
const dbPort = this.configService.get("database.port");
```

## 🧩 Activity / Challenge

1.  Create `src/config/app.config.ts` exporting a factory function.
2.  Return an object `{ app: { name: 'My Nest App' } }`.
3.  Load it in `AppModule`.
4.  Inject `ConfigService` and log `configService.get('app.name')`.

## 🔑 Key Takeaways

- `load`: Allows loading arbitrary configuration objects.
- Supports **Nested Objects** (unlike `.env`).
- Accessed via dot-notation: `get('parent.child')`.
- Good for grouping related settings (e.g., `database`, `auth`, `redis`).
