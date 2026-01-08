# 4. Diving Into e2e Tests

## 🎯 Learning Goal

Understand the difference between Unit Tests and End-to-End (e2e) Tests, and explore the `test` directory layout.

## 🧠 Concept

**Unit Tests**:

- Focus: Single Class (Controller/Service).
- Dependencies: Mocked.
- Speed: Fast ⚡.
- Scope: "Does this function logic work?"

**E2E Tests**:

- Focus: Whole Application.
- Dependencies: Real (or emulated/dockerized).
- Speed: Slower 🐢.
- Scope: "Does the API endpoint `/cats` return 200 OK and the correct JSON?"

NestJS configuration usually separates these. Unit tests sit next to the code (`src/cats/cats.spec.ts`). E2E tests sit in a dedicated `test` folder at the root.

## 💻 Implementation

### The E2E Directory

```
project-root/
├── src/
│   └── cats/
│       └── cats.spec.ts  (Unit)
├── test/
│   ├── app.e2e-spec.ts   (E2E)
│   └── jest-e2e.json     (Config)
```

The `test/jest-e2e.json` config tells Jest to look only for files ending in `.e2e-spec.ts`.

## 🧩 Activity / Challenge

1.  Look at your project folder structure.
2.  Open `test/jest-e2e.json`. Note how the `regex` is different from the main `package.json` jest config.
3.  Run `npm run test:e2e`. This runs the tests in this folder.

## 🔑 Key Takeaways

- **Unit Tests**: Low level, mocked, logic verification.
- **E2E Tests**: High level, real HTTP requests, system verification.
- Separating them allows you to run fast unit tests on every save, and slower E2E tests before commit/deploy.
