# 12. Chapter 4 - Review Quiz

## 📝 Test Your Knowledge

**1. Dependency Injection is primarily used to:**

- [ ] A. Make code run faster
- [x] B. Achieve Inversion of Control and loose coupling
- [ ] C. Encrypt database connections

**2. By default, a provider in a Module is:**

- [ ] A. Public (visible to all modules)
- [x] B. Private (encapsulated)
- [ ] C. Global

**3. Which property allows you to make a provider public?**

- [ ] A. `public: []`
- [ ] B. `imports: []`
- [x] C. `exports: []`

**4. The syntax `providers: [UserService]` is shorthand for:**

- [x] A. `{ provide: UserService, useClass: UserService }`
- [ ] B. `{ provide: UserService, useValue: UserService }`
- [ ] C. `{ provide: 'UserService', useClass: UserService }`

**5. How do you inject a String Token 'API_KEY'?**

- [ ] A. `constructor(private apiKey: 'API_KEY')`
- [x] B. `constructor(@Inject('API_KEY') private apiKey: string)`
- [ ] C. `constructor(@Optional('API_KEY') private apiKey: string)`

**6. If a `useFactory` function returns a Promise, NestJS will:**

- [ ] A. Throw an error
- [x] B. Wait for the promise to resolve before starting the app
- [ ] C. Ignore the promise and return null

**7. Which scope creates a new instance for every single HTTP request?**

- [ ] A. `Scope.DEFAULT`
- [ ] B. `Scope.TRANSIENT`
- [x] C. `Scope.REQUEST`

---

## ✅ Answers

1.  **B** - Loose coupling and IoC.
2.  **B** - Private by default.
3.  **C** - `exports` array.
4.  **A** - Standard class provider.
5.  **B** - Use `@Inject()` decorator for non-class tokens.
6.  **B** - Async providers wait for resolution.
7.  **C** - Request Scope.
