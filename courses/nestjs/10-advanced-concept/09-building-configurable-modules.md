# 9. Building Configurable Modules

## 🎯 Learning Goal

Reuse standard logic to create strongly-typed `registerAsync` methods easily.

## 🧠 Concept

Writing `register()`, `registerAsync()`, `forRoot()`, `forRootAsync()` manually involves a lot of boilerplate (tokens, imports, providers...).
NestJS provides a helper: `ConfigurableModuleBuilder`.

## 💻 Implementation

```typescript
// config.definition.ts
import { ConfigurableModuleBuilder } from '@nestjs/common';

export interface MyOptions {
  apiKey: string;
}

export const {
  ConfigurableModuleClass,
  MODULE_OPTIONS_TOKEN,
} = new ConfigurableModuleBuilder<MyOptions>()
    .setClassMethodName('forRoot')
    .build();

// my.module.ts
@Module({ ... })
export class MyModule extends ConfigurableModuleClass {}

// my.service.ts
@Inject(MODULE_OPTIONS_TOKEN) private options: MyOptions
```

## 🧩 Activity / Challenge

1.  Use the helper to generate a `PaymentModule` with options: `currency`.
2.  Import `PaymentModule.forRoot({ currency: 'USD' })` in AppModule.
3.  Inject the options in a service and log the currency.

## 🔑 Key Takeaways

- **Boilerplate Killer**: Reduces 50 lines of dynamic module code to 5.
- **Consistency**: Ensures all your modules follow NestJS best practices (`forRoot`, `register` naming conventions).
