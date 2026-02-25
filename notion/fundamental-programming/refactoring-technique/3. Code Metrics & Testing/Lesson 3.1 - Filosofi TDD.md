# 💡 CMT03.1: Filosofi TDD

**Outline:**

- **The "Smell" (SEEI):** Writing code that is difficult or impossible to test because tests are written as an afterthought.
- **The "Refactor" (PPP):** Introducing Test-Driven Development (TDD) as a way to use tests to _drive_ a better code design.
- **Your Mission (Production):** Write a failing test for a function that doesn't exist yet.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The "Smell" (SEEI)**

**The Code Smell: Test as an Afterthought**
"Test-Last Development" is a common process smell. You write feature code first, then try to "add" tests. This usually reveals that your code is tightly coupled and not designed for testing. Writing tests then becomes a frustrating chore, often eventually skipped.

**The Negative Impact: Untestable Code and Poor Design**

- **Hard-to-Test Code:** Without thinking about testing during writing, you create oversized classes/functions that are hard to isolate.
- **Tight Coupling:** Code becomes welded to concrete dependencies, making mocks/stubs impossible to use.
- **Slow Feedback:** Design flaws are only found after all code is written, making them expensive to fix.

**Example of the Smell: Trying to Test Afterward**

> **Developer:** "OrderProcessor is done. Now for tests... wait. It directly calls `Database.connect()`, `StripeAPI.charge()`, and `EmailService.send()`. I can't test pricing logic without running those. This needs a huge refactor."

### **Part 2: Practice - The "Refactor" (PPP)**

**The Technique: Embracing Test-Driven Development (TDD)**
TDD makes tests the very first thing you write. It's a design discipline, not just a testing method.

1. **RED Phase (Write a Failing Test):** Write an automated test for functionality that doesn't exist. Run it and confirm it fails. This proves the test works and the feature is missing.
2. **GREEN Phase (Make it Pass):** Write the absolute **minimum** code needed to pass the test. Don't overengineer.
3. **REFACTOR Phase (Clean Up):** Now that you have a safety net, improve the design, remove duplication, and fix names.

**Step-by-Step Implementation: Creating a `slugify` function**
Goal: "Hello World" -> "hello-world"

1. **RED Phase:**

```ts
// slugify.test.ts
test("convert string to slug", () => {
  const result = slugify("Hello World"); // Doesn't exist yet
  expect(result).toBe("hello-world");
});
```

2. **GREEN Phase:**

```ts
// slugify.ts
export function slugify(text: string): string {
  return text.toLowerCase().replace(" ", "-");
}
```

3. **REFACTOR Phase:** Clean up logic or add new failing tests for edge cases (double spaces, etc.).

## 🧠 **Real-World Case Study: "Before vs. After TDD"**

- **Before (Test-Last):**
  > A team builds a module for a week, then finds it untestable. They spend another week on a risky refactor.
- **After (Test-First):**
  > Each step is guided by a test. Design emerges organically. Code is testable from day one. Feature finished in a week with high confidence.
- **The Improvement:** Rapid design feedback ensures decoupled components and better APIs. Testing becomes a design tool, not a chore.

## 🤔 **Reflective Questions**

1. Why must you see a test fail at least once?
2. How does TDD prevent writing unnecessary code?
3. What is the biggest hurdle for new TDD adopters?

## 📚 **Further Reading**

- **Kent Beck:** "Test Driven Development: By Example."
- **Martin Fowler:** TDD summary on his blog.

## 📝 **Mini Task (Production)**

Perform the **RED phase** of TDD for an `isEven` function.

1. Import `isEven` from a non-existent `./calculator`.
2. Write a `test` asserting `isEven(2)` is `true`.
3. Write a `test` asserting `isEven(3)` is `false`.
4. Run and confirm failure.
