# 💡 CMT01.3: Code Coverage: Metrik yang Berguna Namun Berbahaya

**Outline:**

- **The Smell (SEEI):** The fallacy of using 100% code coverage as a sign of perfect testing.
- **The "Refactor" (PPP):** Using coverage analysis not as a goal, but as a tool to discover _untested_ code.
- **Your Refactoring Mission (Production):** Analyze a test with 100% coverage and identify what it's failing to test.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Smell (SEEI)**

**The Code Smell: The "100% Coverage" Fallacy**
Code coverage measures what percentage of your codebase is executed by tests. The smell is the blind pursuit of high coverage as a proxy for quality. **100% coverage does not mean your code is bug-free.** It only means the lines were _executed_, not that the _behavior_ was correctly verified.

**The Negative Impact: A False Sense of Security**

- **Weak Tests:** Developers write tests that run code without meaningful assertions just to bump the numbers.
- **Ignoring Edge Cases:** A test can cover the "happy path" (100% lines) but miss null inputs or divide-by-zero errors.
- **Brittle/Useless Tests:** Tests become coupled to implementation rather than behavior, increasing maintenance cost.
- **Wasted Effort:** Time spent on trivial 100% coverage instead of critical business logic.

**Example of the Smell: The `divide` function**

```ts
function divide(a: number, b: number): number {
  return a / b;
}

// 100% coverage, but poor testing
test("divide should return a result", () => {
  const result = divide(10, 2);
  expect(result).toBeDefined(); // Very weak assertion!
});
```

This test misses result correctness and the divide-by-zero edge case.

### **Part 2: Practice - The "Refactor" (PPP)**

**The Technique: Use Coverage as a Discovery Tool**
Coverage should be a diagnostic tool to show you what you have _not_ tested.

1. **Write Behavioral Tests:** Focus on the "happy path" and critical edge cases first.
2. **Run Tests and Analyze Coverage:** Generate a report.
3. **Investigate the Gaps:** Look at the "red" (uncovered) lines. Ask "why did I miss this scenario?"
4. **Write a New Test:** Specifically target that gap.

**Step-by-Step Implementation: Better `divide` tests**

```ts
test("divide should return the correct quotient", () => {
  expect(divide(10, 2)).toBe(5);
});
test("divide should throw an error when dividing by zero", () => {
  expect(() => divide(10, 0)).toThrow("Division by zero");
});
```

100% coverage is now a _byproduct_ of good testing, providing actual confidence.

## 🧠 **Real-World Case Study: "Before vs. After Mindset"**

- **Before (Coverage as a Goal):**
  > "My PR is blocked at 83% coverage. I'll add some junk tests to hit the 85% requirement."
  - **Result:** Wasted time, annoying process, no quality gain.
- **After (Coverage as a Tool):**
  > "Coverage shows my error handling for failed API calls isn't tested. I'll add a mock test for that scenario."
  - **Result:** Found a genuine gap, added a valuable test, increased system resilience.

## 🤔 **Reflective Questions**

1. Line coverage vs. Branch coverage: Which is stronger?
2. 70% behavior-driven coverage vs. 95% assertion-less coverage: Which do you trust?
3. Should teams fail builds based on coverage thresholds?

## 📚 **Further Reading**

- **Martin Fowler:** [TestCoverage](https://martinfowler.com/bliki/TestCoverage.html).
- **Testing Trophy:** An alternative to the pyramid emphasizing value over quantity.
- **Jest documentation:** Configuring and interpreting coverage reports.

## 📝 **Mini Task (Production)**

Analyze the following function and its 100% coverage test.

1. Explain why it is poorly tested despite 100% coverage.
2. Identify two missing edge cases.
3. Write the Jest `test(...)` blocks (descriptions only) for the gaps.

```ts
function getAbbreviatedName(fullName: string): string {
  const parts = fullName.trim().split(" ");
  const firstName = parts[0];
  const lastName = parts.length > 1 ? parts[parts.length - 1] : "";

  if (lastName) return `${firstName} ${lastName.charAt(0)}.`;
  return firstName;
}

// 100% coverage tests
describe("getAbbreviatedName", () => {
  test("standard name", () =>
    expect(getAbbreviatedName("John Smith")).toBe("John S."));
  test("single name", () => expect(getAbbreviatedName("Cher")).toBe("Cher"));
});
```
