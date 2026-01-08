# 5. Creating our First e2e Test

## 🎯 Learning Goal

Set up the `app.e2e-spec.ts` boilerplate to bootstrap the full NestJS application structure for testing.

## 🧠 Concept

Unlike unit tests where we use `.compile()`, for E2E tests we use `.init()` to actually start the Nest application instance.
We also use a library called **Supertest** to simulate HTTP requests against this running instance.

## 💻 Implementation

### 1. Setup Boilerplate

This looks similar to a Unit Test, but we are creating the _entire_ `AppModule`.

```typescript
import { Test, TestingModule } from "@nestjs/testing";
import { INestApplication } from "@nestjs/common";
import * as request from "supertest";
import { AppModule } from "./../src/app.module";

describe("AppController (e2e)", () => {
  let app: INestApplication;

  beforeEach(async () => {
    // 1. Compile the Module (Root Module)
    const moduleFixture: TestingModule = await Test.createTestingModule({
      imports: [AppModule],
    }).compile();

    // 2. Create Nest App Instance
    app = moduleFixture.createNestApplication();

    // 3. Initialize (Bootstraps logic like onModuleInit)
    await app.init();
  });

  // ... tests go here ...
});
```

## 🧩 Activity / Challenge

1.  Locate `test/app.e2e-spec.ts`.
2.  If it exists, delete the content and try to type it out from scratch (muscle memory!).
3.  Pay attention to the imports. `AppModule` is imported from `../src/app.module`.

## 🔑 Key Takeaways

- We import the **Root Module** (`AppModule`), ensuring the test runs with the exact same configuration as the real app.
- `app.init()` triggers the startup sequence.
- `app.getHttpServer()` will be passed to Supertest to assign a transient port.
