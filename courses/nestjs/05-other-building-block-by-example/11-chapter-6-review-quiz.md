# 11. Chapter 6 - Review Quiz

## 📝 Test Your Knowledge

**1. Which block executes FIRST when a request arrives?**

- [ ] A. Guard
- [ ] B. Interceptor
- [ ] C. Middleware

**2. Which block is best suited for Authorization (checking permissions)?**

- [ ] A. Pipe
- [ ] B. Guard
- [ ] C. Filter

**3. If you want to transform the RESPONSE sent to the user, what should you use?**

- [ ] A. Pipe
- [ ] B. Interceptor (rxJS map)
- [ ] C. Middleware

**4. Where do you typically bind global middleware?**

- [ ] A. In `main.ts` `app.use()`
- [ ] B. In `@Module({ providers: [] })`
- [ ] C. In `AppModule` class implementing `NestModule`

**5. How do Exceptions Filters access the Request/Response objects?**

- [ ] A. `this.request`
- [ ] B. `ArgumentsHost`
- [ ] C. Dependency Injection only

**6. What does `Reflector` do?**

- [ ] A. Reflects the API calls to a mirror server
- [ ] B. Retrieves metadata attached to classes/methods
- [ ] C. Validates types

---

## ✅ Answers

1.  **C** - Middleware runs before Guards.
2.  **B** - Guards.
3.  **B** - Interceptors (Post-Controller).
4.  **C** - `configure()` method of a NestModule.
5.  **B** - `ArgumentsHost` gives context-agnostic access.
6.  **B** - Retrieves metadata.
