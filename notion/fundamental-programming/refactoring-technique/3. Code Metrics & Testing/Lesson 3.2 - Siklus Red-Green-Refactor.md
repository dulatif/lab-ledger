# 💡 CMT03.2: Siklus Red-Green-Refactor

**Outline:**

- **The "Smell" (SEEI):** Feeling overwhelmed when trying TDD, writing tests that are too large or too much code at once.
- **The "Refactor" (PPP):** Breaking down the TDD workflow into three tiny, repeatable steps: Red, Green, Refactor.
- **Your Mission (Production):** Go through one full Red-Green-Refactor cycle for a simple function.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The "Smell" (SEEI)**

**The Code Smell: "Big Bang" TDD**
Attempting TDD by writing one massive test for a huge feature. You then spend an hour writing code to make it pass, losing the rapid feedback benefit. Taking giant risky leaps instead of small safe steps is a process smell.

**The Negative Impact: Frustration and Getting Lost**

- **No Measurable Progress:** Without a stable "green" state, you don't know if you're actually moving forward.
- **Poor Design:** Giant tests prevent TDD from guiding the design organically step-by-step.
- **Abandoning TDD:** Developers get frustrated and return to "test-last" habits, assuming TDD is too hard.

**Example of the Smell: The Giant Parser Test**

```ts
test("parse complex CSV into user object with validation", () => {
  const user = parseCsvUser("1,John Doe,john@example.com,true");
  expect(user.id).toBe(1);
  expect(user.name).toBe("John Doe");
  expect(user.email).toBe("john@example.com");
  // ... 10 more validations ...
});
```

Making this pass all at once is a monumental and stressful task.

### **Part 2: Practice - The "Refactor" (PPP)**

**The Technique: The Red-Green-Refactor Mantra**
A rapid, tiny cycle that should take minutes, not hours.

1. **RED:** Write the **smallest** failing test for the next logical step. Run it and watch it fail.
2. **GREEN:** Write the **simplest**, even "ugly" code to make that one test pass. Hardcoding is okay here—get to green fast.
3. **REFACTOR:** With the safety net of passing tests, clean up the code. Remove duplication and improve design while staying green.

**Step-by-Step Implementation: Building the CSV Parser**

- **Cycle 1: Parse the ID**
  - **RED:** Test for `parseCsvUser('1,John').id === 1`. (Fails).
  - **GREEN:** Return `{ id: 1 }`. (Passes).
  - **REFACTOR:** `return { id: Number(csv.split(',')[0]) }`. (Passes).
- **Cycle 2: Parse the Name**
  - **RED:** Test for `name === 'John'`. (Fails).
  - **GREEN:** `return { id: ..., name: parts[1] }`. (Passes).

## 🧠 **Real-World Case Study: "Before vs. After the Cycle"**

- **Before (Big Leap):**
  > Developer spends half a day struggling to get a complex `generateReport` function working. They are stuck in a frustrating fail-loop.
- **After (Small Steps):**
  > The same developer uses TDD. They write a test for the header (Green), then for the first row (Green). They make stable progress every few minutes and finish in two hours with confidence.
- **The Improvement:** The cycle turns development into a calm, methodical, and predictable process. You always have a safety net.

## 🤔 **Reflective Questions**

1. Why is "cheating" (hardcoding) okay in the GREEN phase?
2. How does the REFACTOR phase relate to eliminating Code Smells?
3. How does being "always green" reduce cognitive load?

## 📚 **Further Reading**

- **Pragmatic Programmer:** Sections on incremental development and rapid feedback.
- **Refactoring.guru:** Use these techniques during the REFACTOR phase.

## 📝 **Mini Task (Production)**

Build a `sum` function for an array.

1. **RED:** Write a test asserting `sum([1, 2, 3])` returns `6`. Confirm it fails.
2. **GREEN:** Write the simplest implementation to make it pass.
3. **REFACTOR:** Clean up the implementation (e.g., using `reduce`) while keeping the test green.
