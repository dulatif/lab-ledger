# 7. Chapter 8 - Review Quiz

## 📝 Test Your Knowledge

**1. Which command runs unit tests in NestJS?**

- [ ] A. `npm run e2e`
- [x] B. `npm test`
- [ ] C. `node test.js`

**2. What describes a "Unit Test"?**

- [ ] A. Testing the entire application flow from HTTP to Database
- [x] B. Testing a single class in isolation, mocking dependencies
- [ ] C. Testing the UI

**3. In a Unit Test, how do you provide a fake implementation for a Service?**

- [ ] A. You can't, you must use the real one
- [x] B. `{ provide: Service, useValue: mockObject }`
- [ ] C. `new Service()`

**4. Where do E2E tests usually live in a standard NestJS project?**

- [ ] A. `src/e2e`
- [x] B. `tests/` (root level)
- [ ] C. `dist/`

**5. What library is used to simulate HTTP requests in E2E tests?**

- [ ] A. Axios
- [x] B. Supertest
- [ ] C. Express

---

## ✅ Answers

1.  **B** - `npm test`.
2.  **B** - Isolation is key.
3.  **B** - `useValue` (or `useClass`).
4.  **B** - `test/` folder at the root.
5.  **B** - Supertest.
