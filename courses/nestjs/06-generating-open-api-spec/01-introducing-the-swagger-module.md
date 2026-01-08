# 1. Introducing the Swagger Module

## 🎯 Learning Goal

Install and configure the `@nestjs/swagger` module to automatically generate interactive API documentation for your application.

## 🧠 Concept

**OpenAPI** (formerly Swagger) is a specification for describing RESTful APIs.
Instead of manually writing a PDF or Wiki that gets outdated instantly 👴, NestJS can inspect your decorators (`@Get`, `@Post`, DTOs) and generate the documentation **live**.

## 💻 Implementation

### 1. Installation

You need the module and `swagger-ui-express` (the visual interface).

```bash
npm install --save @nestjs/swagger swagger-ui-express
```

### 2. Setup in main.ts

We insert the Swagger setup code **before** `app.listen()`.

```typescript
// src/main.ts
import { NestFactory } from "@nestjs/core";
import { SwaggerModule, DocumentBuilder } from "@nestjs/swagger";
import { AppModule } from "./app.module";

async function bootstrap() {
  const app = await NestFactory.create(AppModule);

  // 1. Build the config
  const config = new DocumentBuilder()
    .setTitle("Cats example")
    .setDescription("The cats API description")
    .setVersion("1.0")
    .addTag("cats")
    .build();

  // 2. Create the document
  const document = SwaggerModule.createDocument(app, config);

  // 3. Mount the Swagger UI (e.g. at /api)
  SwaggerModule.setup("api", app, document);

  await app.listen(3000);
}
bootstrap();
```

## 🧩 Activity / Challenge

1.  Install the packages.
2.  Add the bootstrap code to your `main.ts`.
3.  Start your app (`npm run start:dev`).
4.  Open your browser to `http://localhost:3000/api`.
5.  You should see the Swagger UI!

## 🔑 Key Takeaways

- `DocumentBuilder`: Helper to structure the base OpenAPI info.
- `SwaggerModule.createDocument`: Generates the JSON spec.
- `SwaggerModule.setup`: Serves the UI.
