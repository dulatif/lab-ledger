# 1. Introduction to Jest

## 🎯 Learning Goal

Understand why we use **Jest** and learn the basic syntax for writing tests (`describe`, `it`, `expect`).

## 🧠 Concept

NestJS ships with **Jest** pre-configured. Jest is a delightful JavaScript Testing Framework with a focus on simplicity.
It provides:

- **Test Runner**: Finds and runs your tests.
- **Assertion Library**: Checks if values meet expectations (`expect(x).toBe(y)`).
- **Mocking Support**: Faking dependencies.

## 💻 Implementation

### Basic Syntax

Tests are usually written in files ending with `.spec.ts` (unit) or `.e2e-spec.ts` (end-to-end).

```typescript
// cats.service.spec.ts

// 1. Group related tests
describe("CatsService", () => {
  // 2. Define a single test case
  it("should be clean", () => {
    // 3. Assertion (Expectation)
    const isClean = true;
    expect(isClean).toBe(true);
  });

  it("should verify math", () => {
    expect(1 + 1).toEqual(2);
  });
});
```

## 🧩 Activity / Challenge

1.  Create a file `math.spec.ts`.
2.  Write a test suite `describe('Math')`.
3.  Add a test `it('should add numbers')`.
4.  Assert that `2 + 3` equals `5`.
5.  Run `npm test` and watch it pass! (NestJS default configuration runs all `*.spec.ts` files).

## 🔑 Key Takeaways

- **describe**: Groups tests (usually by Class or Method name).
- **it** (or `test`): The actual test case. Should describe _what_ it is testing.
- **expect**: The entry point for assertions.
- Run tests with `npm test` (single run) or `npm run test:watch` (updates on save).
