# 💡 CMT02.1: Pengantar Jest untuk TypeScript

**Outline:**

- **The "Smell" (SEEI):** The fear of change and the risk of working with untested code.
- **The "Refactor" (PPP):** Introducing a testing framework (Jest) as a safety net to enable confident refactoring.
- **Your Mission (Production):** Write your first Jest test for a simple utility function.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The "Smell" (SEEI)**

**The Code Smell: Fear of Change**
An unhealthy codebase is defined by fear. Developers are afraid to refactor or add features because they lacks a safety net. Without automated tests, every small change risks breaking a distant part of the application. Code without tests is, by definition, legacy code.

**The Negative Impact: Technical Debt and Stagnation**

- **No Refactoring:** Improvements are postponed indefinitely as the risk of breaking functionality is too high.
- **Slow Development:** Every change requires tedious manual testing.
- **Bugs in Production:** Without verification, bugs inevitably slip through, damaging trust and morale.

**Example of the Smell: The "Don't Touch It!" Module**

> **New Developer:** "I think I can optimize this `calculatePrice` function."
> **Senior Developer:** "Don't touch it! It works. The last person who changed it broke checkout for two days. We have no tests, so leave it alone."

### **Part 2: Practice - The "Refactor" (PPP)**

**The Technique: Build a Safety Net with Jest**
Unit tests verify that small, isolated pieces of code behave as expected. We use **Jest** for its speed and rich features in TypeScript.

1. **Setup:** Use `ts-jest` for TypeScript projects.
2. **Structure:** Use global functions:
   - `describe(name, fn)`: Groups related tests (a "suite").
   - `test(name, fn)`: Defines a specific test case.
   - `expect(value)`: The entry point for assertions.
3. **Assert:** Use "matchers" like `.toBe()`, `.toEqual()`, or `.toBeTruthy()`.

**Step-by-Step Implementation: Testing an `add` Function**

```ts
// math.ts
export const add = (a: number, b: number) => a + b;

// math.test.ts
import { add } from "./math";

describe("add", () => {
  it("sum of two positive numbers", () => {
    expect(add(2, 3)).toBe(5);
  });
  it("sum of positive and negative", () => {
    expect(add(5, -2)).toBe(3);
  });
});
```

Run tests with `npx jest`.

## 🧠 **Real-World Case Study: "Before vs. After Confidence"**

- **Before (No Tests):**
  > Developer adds a `subtract` function but worries it might break `add`. They must manually test everything.
- **After (With Tests):**
  > Developer adds the feature, runs `npx jest`, and sees `add` still passes. They proceed with instant confidence.
- **The Improvement:** The suite acts as a regression net. Fear is replaced by automated verification.

## 🤔 **Reflective Questions**

1. Difference between `.toBe()` and `.toEqual()`?
2. Why group tests with `describe`?
3. What is the terminal command to run Jest?

## 📚 **Further Reading**

- **Jest Docs:** [Getting Started](https://jestjs.io/docs/getting-started) guide.
- **ts-jest:** Configuration for TypeScript projects.

## 📝 **Mini Task (Production)**

Create `string-utils.test.ts` for the `capitalize` function below.

```ts
// string-utils.ts
export function capitalize(str: string): string {
  if (!str) return "";
  return str.charAt(0).toUpperCase() + str.slice(1);
}
```

**Your Mission:**

1. Write a `describe` suite for `capitalize`.
2. Write three `it` cases: lowercase string ('hello'), capitalized string ('World'), and empty string ('').
