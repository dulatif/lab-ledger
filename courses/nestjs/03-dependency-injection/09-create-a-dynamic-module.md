# 9. Create a Dynamic Module

## 🎯 Learning Goal

Learn how to create modules that can be configured from the outside (like passing a database URL or API key when importing).

## 🧠 Concept

Static modules (like `CatsModule`) are fixed. You import them, and they behave the same way every time.
**Dynamic Modules** allow you to pass arguments **when** you import them. 🏎️

This uses a static method (conventionally named `register` or `forRoot`) that returns a `DynamicModule` object.

## 💻 Implementation

### 1. Define the Options Interface

First, what options do we want to pass?

```typescript
export interface ConfigOptions {
  folder: string;
}
```

### 2. Create the Static Register Method

Inside your module:

```typescript
import { Module, DynamicModule } from "@nestjs/common";
import { ConfigService } from "./config.service";

@Module({})
export class ConfigModule {
  static register(options: ConfigOptions): DynamicModule {
    return {
      module: ConfigModule,
      providers: [
        {
          provide: "CONFIG_OPTIONS",
          useValue: options,
        },
        ConfigService,
      ],
      exports: [ConfigService],
    };
  }
}
```

### 3. Consume the Dynamic Module

Now, in `AppModule`, we don't just say `ConfigModule`. We call the function!

```typescript
@Module({
  imports: [ConfigModule.register({ folder: "./config" })],
})
export class AppModule {}
```

### 4. Use the Options

The `ConfigService` can now inject `'CONFIG_OPTIONS'` to know where to look.

```typescript
@Injectable()
export class ConfigService {
  constructor(@Inject("CONFIG_OPTIONS") private options: ConfigOptions) {}
}
```

## 🧩 Activity / Challenge

1.  Create a `DatabaseModule`.
2.  Add a `forRoot(url: string)` static method.
3.  The method should return a Dynamic Module that provides a string token `'DB_URL'` with the value of the argument.
4.  Import `DatabaseModule.forRoot('localhost')` in your `AppModule`.
5.  Inject `'DB_URL'` into a service and log it.

## 🔑 Key Takeaways

- **Dynamic Modules** allow modules to accept configuration arguments.
- They return a `DynamicModule` object (which looks like a standard Module decorator object + a `module` property).
- Common naming conventions:
  - `register()`: For configuring a specific module.
  - `forRoot()`: For configuring a module once for the entire app (usually Core modules).
