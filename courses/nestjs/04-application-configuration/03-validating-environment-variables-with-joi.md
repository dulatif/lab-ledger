# 3. Validating Environment Variables with Joi

## 🎯 Learning Goal

Prevent your app from starting is required environment variables are missing or invalid using Joi validation.

## 🧠 Concept

If you forget to set `DATABASE_URL` in production, your app might start successfully but crash 5 minutes later when a user tries to log in. 😱
**Fail Fast!** It is better to crash immediately at startup with a clear error message.

We can use the `joi` library to define a schema for our environment variables.

## 💻 Implementation

### 1. Install Joi

```bash
npm install --save joi
```

### 2. Define Validation Schema

Pass the `validationSchema` property to `forRoot`.

```typescript
import * as Joi from "joi";

ConfigModule.forRoot({
  validationSchema: Joi.object({
    NODE_ENV: Joi.string()
      .valid("development", "production", "test")
      .default("development"),
    PORT: Joi.number().default(3000),
    DATABASE_USER: Joi.string().required(), // 🚨 App will fail if missing!
  }),
});
```

### 3. Defaults

Notice `.default(3000)`. If `PORT` is missing in the `.env` file, Joi will fill it in for us automatically!

## 🧩 Activity / Challenge

1.  Install `joi`.
2.  Add a validation schema requiring a value for `API_KEY`.
3.  Remove `API_KEY` from your `.env` file.
4.  Try to start the app.
5.  Observe the error message in the console.

## 🔑 Key Takeaways

- **Fail Fast**: Validation ensures the app has everything it needs before it starts handling traffic.
- **Joi**: A powerful schema description language and data validator for JavaScript.
- `validationSchema`: The NestJS Config option to hook up Joi.
