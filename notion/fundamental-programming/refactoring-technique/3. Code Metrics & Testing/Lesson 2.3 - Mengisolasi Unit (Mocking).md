# 💡 CMT02.3: Mengisolasi Unit (Mocking)

**Outline:**

- **The "Smell" (SEEI):** Tests that are slow, unreliable, and not true "unit" tests because they depend on external systems (databases, APIs, etc.).
- **The "Refactor" (PPP):** Introducing Test Doubles (Stubs and Mocks) to isolate the unit under test.
- **Your Mission (Production):** Analyze a function and determine when to use a stub vs. a mock.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The "Smell" (SEEI)**

**The Code Smell: The Test That Isn't a Unit Test**
A unit test must test a single unit of code in **isolation**. If your test requires a live database or a third-party API, it's an **integration test**. These are often slow and flaky, failing due to network issues rather than code bugs.

**The Negative Impact: The Unreliable Test Suite**

- **Slow:** Network/DB calls take seconds, slowing down the feedback loop.
- **Flaky:** External dependency failures cause developers to ignore tests.
- **Hard to Write:** It's difficult to force a real API to return specific errors for testing edge cases.

**Example of the Smell: Testing a User Service**

```ts
class UserService {
  private db: Database;
  public getUserFullName(userId: number): string {
    const user = this.db.query(`SELECT * FROM users WHERE id = ${userId}`);
    return `${user.firstName} ${user.lastName}`;
  }
}

// Brittle integration test
test("get full name", () => {
  const service = new UserService();
  expect(service.getUserFullName(1)).toBe("John Doe");
});
```

### **Part 2: Practice - The "Refactor" (PPP)**

**The Technique: Using Test Doubles**
Replace real dependencies with fake objects called **Test Doubles**.

1. **Stub (State Provider):**
   - **Purpose:** Provide canned data.
   - **Focus: State**. You just care that it returns the right data when called.
   - **Example:** A DB stub that always returns a fake user object.
2. **Mock (Interaction Verifier):**
   - **Purpose:** Verify interactions.
   - **Focus: Behavior**. You care _if_ and _how_ a method was called.
   - **Example:** An email mock verifying that `send()` was called with the correct address.

**Step-by-Step Implementation: Stubbing the Database**

```ts
class StubDatabase {
  public query(sql: string) {
    return { firstName: "Stubbed", lastName: "User" };
  }
}

test("get full name using stub", () => {
  const stubDb = new StubDatabase();
  const service = new UserService(stubDb); // Dependency Injection
  expect(service.getUserFullName(999)).toBe("Stubbed User");
});
```

## 🧠 **Real-World Case Study: "Before vs. After Isolation"**

- **Before (Slow Integration Test):**
  > `OrderProcessor` test makes a real API call. Takes 2 seconds and fails during maintenance windows.
- **After (Fast Unit Test):**
  > `OrderProcessor` uses a `PaymentService` stub. Tests run in milliseconds. A mock verifies that `chargeCard()` was called with the correct amount.
- **The Improvement:** Instant feedback and stable tests. Trust in the suite is restored.

## 🤔 **Reflective Questions**

1. What is the main difference between a stub and a mock?
2. When would you use a mock instead of a stub?
3. Why is Dependency Injection (DI) crucial for testability?

## 📚 **Further Reading**

- **Martin Fowler:** [TestDouble](https://martinfowler.com/bliki/TestDouble.html).
- **Sinon.JS documentation:** For a deep dive into different types of doubles.

## 📝 **Mini Task (Production)**

You have a `UserRepo` that saves a user and sends an email via a `Notifier`.

```ts
export class UserRepo {
  constructor(private notifier: Notifier) {}
  saveUser(email: string) {
    // ... save logic ...
    this.notifier.send(email, "Account created.");
  }
}
```

**Your Mission:**
Explain how you would test `saveUser`.

1. Would you use a **stub** or a **mock** for `Notifier`?
2. Why? What specifically would you verify?
