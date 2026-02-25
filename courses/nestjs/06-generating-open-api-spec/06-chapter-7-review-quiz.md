# 6. Chapter 7 - Review Quiz

## 📝 Test Your Knowledge

**1. Which configuration file do you modify to enable the Swagger CLI Plugin?**

- [ ] A. `package.json`
- [ ] B. `tsconfig.json`
- [x] C. `nest-cli.json`

**2. What is the primary benefit of the CLI Plugin?**

- [ ] A. It makes the build faster
- [x] B. It automatically adds `@ApiProperty` decorators, so you don't have to manualy type them
- [ ] C. It generates a PDF

**3. If you want to group all "Users" endpoints together in the component UI, which decorator do you use?**

- [ ] A. `@ApiGroup('users')`
- [x] B. `@ApiTags('users')`
- [ ] C. `@ApiFolder('users')`

**4. Where do you initialize the Swagger Module?**

- [ ] A. `AppModule`
- [ ] B. `SwaggerController`
- [x] C. `main.ts` (bootstrap function)

**5. How do you document that an endpoint might return a 404 error?**

- [x] A. `@ApiNotFoundResponse()`
- [ ] B. `@ApiError(404)`
- [ ] C. You can't, Swagger only shows successful responses

---

## ✅ Answers

1.  **C** - `nest-cli.json`.
2.  **B** - Automation of metadata.
3.  **B** - `@ApiTags`.
4.  **C** - `main.ts`.
5.  **A** - `@ApiNotFoundResponse`.
