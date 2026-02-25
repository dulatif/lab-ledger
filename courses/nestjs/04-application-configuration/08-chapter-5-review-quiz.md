# 8. Chapter 5 - Review Quiz

## 📝 Test Your Knowledge

**1. Which package provides the standard way to handle .env files in NestJS?**

- [ ] A. `dotenv`
- [x] B. `@nestjs/config`
- [ ] C. `@nestjs/environment`

**2. Why should you use `ConfigService` instead of `process.env`?**

- [x] A. It provides type safety and better testability
- [ ] B. It is faster
- [ ] C. `process.env` is deprecated in Node.js

**3. What does `Joi` validation schema do in ConfigModule?**

- [ ] A. Encrypts the environment variables
- [ ] B. Validates variables at runtime for every request
- [x] C. Validates variables at startup and throws an error if validation fails

**4. How do you create a namespaced configuration slice?**

- [ ] A. `createConfig('name', ...)`
- [ ] B. `ConfigModule.namespace('name')`
- [x] C. `registerAs('name', ...)`

**5. Which method allows you to inject ConfigService into another module's configuration (like TypeORM)?**

- [ ] A. `TypeOrmModule.forRoot()`
- [x] B. `TypeOrmModule.forRootAsync()`
- [ ] C. `TypeOrmModule.withConfig()`

**6. If you have a `.env` file and a `configuration.ts` custom file, which value takes precedence and overwrites the other?**

- [x] A. `.env` overwrites custom config
- [ ] B. Custom config overwrites `.env`
- [ ] C. They are merged and custom config wins on conflict

---

## ✅ Answers

1.  **B** - `@nestjs/config` (which uses dotenv under the hood).
2.  **A** - Types, mocking, and encapsulation.
3.  **C** - Fails fast at startup.
4.  **C** - `registerAs`.
5.  **B** - `.forRootAsync()`.
6.  **A** - Environment variables (and .env) have higher priority than loaded custom file values in the default hierarchy.
