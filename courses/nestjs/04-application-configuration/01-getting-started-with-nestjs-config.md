# 1. Getting Started with NestJS Config

## 🎯 Learning Goal

Learn how to install and set up the `@nestjs/config` module to manage environment variables instead of hardcoding values.

## 🧠 Concept

Hardcoding secrets (like API keys or DB passwords) in your code is a **security risk** 🛑.
The standard practice is to use **Environment Variables**.
NestJS provides a dedicated package that wraps the popular `dotenv` library, treating configuration as a **dependency** that you can inject into your app.

## 💻 Implementation

### 1. Installation

```bash
npm install --save @nestjs/config
```

### 2. Basic Setup

Import `ConfigModule` in your root `AppModule`.
The `.forRoot()` method initializes the module and loads your `.env` file by default.

```typescript
// src/app.module.ts
import { Module } from "@nestjs/common";
import { ConfigModule } from "@nestjs/config";

@Module({
  imports: [
    ConfigModule.forRoot(), // 👈 Load .env
  ],
})
export class AppModule {}
```

### 3. Creating the .env file

Create a file named `.env` in your **project root** (not inside `src`).

```env
DATABASE_USER=admin
DATABASE_PASSWORD=secret
PORT=3000
```

> [!IMPORTANT]
> Always add `.env` to your `.gitignore` file! You should never commit secrets to Git.

## 🧩 Activity / Challenge

1.  Install `@nestjs/config`.
2.  Add `ConfigModule.forRoot()` to `AppModule`.
3.  Create a `.env` file with `MESSAGE=Hello Config`.
4.  (Temporary test) In `main.ts`, try logging `process.env.MESSAGE` to verify it loaded.

## 🔑 Key Takeaways

- Use `@nestjs/config` to manage configuration.
- `ConfigModule.forRoot()` loads the `.env` file from the root directory.
- **Never commit .env files**.
