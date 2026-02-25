# 💡 CMT03.3: Piramida Pengujian

**Outline:**

- **The "Smell" (SEEI):** The "Ice Cream Cone" anti-pattern, where the test suite is dominated by slow, brittle End-to-End (E2E) tests.
- **The "Refactor" (PPP):** Introducing the Testing Pyramid as a model for a healthy and efficient testing strategy.
- **Your Mission (Production):** Categorize a series of test scenarios into the correct layer of the pyramid.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The "Smell" (SEEI)**

**The Code Smell: The "Ice Cream Cone" Anti-Pattern**
A smell in your testing strategy. Few fast unit tests, some integration tests, and a massive number of E2E tests at the top. E2E tests (automating browser/app) are slow, fail for unreliable reasons (network blips, slow animations), and are hard to debug. You don't know if a failure is in the frontend, backend, or network.

**The Negative Impact: Slow, Unreliable Feedback**

- **Painfully Slow feedback Loop:** If tests take 45 minutes, developers won't run them often. They'll commit and hope, finding out about breaks much later.
- **High Maintenance Cost:** E2E tests are brittle. Tiny UI changes break dozens of tests.
- **Low Confidence:** "Flaky" tests cause developers to ignore failures. An unreliable suite is worse than no suite.

**Example of the Smell: Relying on UI Tests**

> **PM:** "How do we know login works?"
> **Dev:** "We have an E2E test that opens a browser, types credentials, and clicks login."
> The problem: This could fail for 20 reasons unrelated to login logic (e.g., button color change, unrelated JS bug).

### **Part 2: Practice - The "Refactor" (PPP)**

**The Technique: Build a Testing Pyramid**
Push tests as far down the pyramid as possible for maximum efficiency.

1. **Unit Tests (Pyramid Base):**
   - **Scope:** The majority of tests. Isolated classes/functions, business logic, edge cases.
   - **Characteristics:** Extremely fast, stable, easy to write/debug.
2. **Integration Tests (Pyramid Middle):**
   - **Scope:** Fewer than unit tests.
   - **What they test:** Interaction between components (e.g., Service calling a real DB).
   - **Characteristics:** Slower than unit tests, but still relatively fast.
3. **End-to-End (E2E) Tests (Pyramid Top):**
   - **Scope:** Very few, only critical scenarios.
   - **What they test:** Full user journeys (e.g., login -> checkout).
   - **Characteristics:** Slow, brittle, expensive to maintain.

**Step-by-Step Implementation: Re-testing Login**
Instead of one big E2E test:

- **Unit Tests:** Test `isValidEmail()`, `hashPassword()`, and `UserController` (with mocked services).
- **Integration Test:** Verify `UserService` can fetch from a real development DB.
- **E2E Test:** A single "happy path" login-to-redirect test.

## 🧠 **Real-World Case Study: "Before vs. After the Pyramid"**

- **Before (Ice Cream Cone):**
  > Test suite takes 2 hours. QA spends half a day investigating flaky failures. Releases blocked for days.
- **After (Healthy Pyramid):**
  > 95% of tests are unit/integration (3 minutes). Only 10 critical E2E tests. Feedback is nearly instantaneous.
- **The Improvement:** Efficiency optimization. Fastest feedback for bulk code; slow tests reserved only for wiring verification.

## 🤔 **Reflective Questions**

1. Why are unit tests the base?
2. What can _only_ be verified by an E2E test?
3. How does "pushing tests down" improve design?

## 📚 **Further Reading**

- **Martin Fowler:** [The Practical Test Pyramid](https://martinfowler.com/articles/practical-test-pyramid.html).
- **Google Testing Blog:** Strategies for balanced testing at scale.

## 📝 **Mini Task (Production)**

Categorize these blog feature scenarios into **Unit**, **Integration**, or **E2E**:

- **A:** Verifying `generateSlug()` turns "My New Post" into "my-new-post".
- **B:** Verifying that clicking "Publish" makes the post appear on the homepage.
- **C:** Verifying `PostService` saves data to a real database and matches the schema.

Briefly explain your reasoning for each.
