# 1. Before we get started

## 🎯 Learning Goal

Ensure you have the necessary prerequisites to successfully implement Authentication.

## 🧠 Concept

Authentication is a complex beast. To keep this chapter focused on **NestJS principles** and **Security**, we will assume you have a basic persistence layer ready.

We will rely on a `UsersService` that can `findByEmail()` and `create()`. It doesn't matter if you use TypeORM, Mongoose, or Prisma. The logic remains the same.

## 💻 Implementation

### Prerequisite Checklist

1.  **Database Connection**: Ensure your app is connected to a DB.
2.  **User Entity**: You should have a table/collection defined with at least:
    - `id` (number/uuid)
    - `email` (unique string)
    - `password` (string)
3.  **Core Module**: Familiarity with Modules, Providers, and Controllers.

### Recommended Packages

We will be using:

- `@nestjs/jwt`: For token generation.
- `@nestjs/passport`: The glue library.
- `passport`: The underlying strategies ecosystem.
- `bcrypt`: For password hashing.

```bash
npm install --save @nestjs/jwt @nestjs/passport passport passport-local passport-jwt bcrypt
npm install --save-dev @types/passport-local @types/passport-jwt @types/bcrypt
```

## 🧩 Activity / Challenge

1.  Install the dependencies above.
2.  Review your existing `User` entity or create a simple one if you haven't yet.

## 🔑 Key Takeaways

- Auth is framework-agnostic but relies on a solid DB foundation.
- We use standard industry libraries (`passport`, `bcrypt`) rather than reinventing the wheel.
