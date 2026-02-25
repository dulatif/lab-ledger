# 💡 CMT03.4: Beyond Unit Tests

**Outline:**

- **The Smell (SEEI):** A false confidence that 100% unit test coverage means the application works, only to find bugs in production when different components interact.
- **The Refactor (PPP):** Introducing Integration Tests to verify the "seams" and contracts between different parts of the system.
- **Your Mission (Production):** Design an integration test to verify the interaction between a service and a data repository.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The "Smell" (SEEI)**

**The Code Smell: The Gaps Between the Units**
Even with 100% unit test coverage, apps often crash in production. Why? Because units like `UserService` and `UserRepository` are tested with mocks and never actually talk to each other. Gaps in data expectations or SQL logic go unnoticed because the units never integrated during testing.

**The Negative Impact: Failure at the Seams**

- **Contract Violations:** Bugs happen at boundaries. Unit tests can't verify that two units adhere to the same data format or error-handling "contract."
- **Configuration Issues:** Incorrect DB URLs or invalid API keys are invisible to isolated unit tests.
- **Hard Debugging:** Boundary bugs are often environment-specific and hard to reproduce locally without infrastructure.

**Example of the Smell: The Broken Contract**

```ts
// UserRepository.ts (isolated test passes)
// Returns { user_name: 'John' } instead of { name: 'John' }

// UserService.ts (mocked test passes)
// Mock returns { name: 'John' }.
async getFormattedName(id: number) {
    const user = await this.repo.findById(id);
    return user.name.toUpperCase(); // CRASH in prod: user.name is undefined.
}
```

### **Part 2: Practice - The "Refactor" (PPP)**

**The Technique: Writing Integration Tests**
Integration tests verify workflows spanning multiple units. They focus on **interaction**, not internal implementation.

- **Common Examples:**
  - **API to Service:** Real HTTP endpoint calls service.
  - **Service to Database:** Service reads/writes to a real test DB.
  - **Service to External API:** Service communicates with a sandbox API.

**Step-by-Step Implementation: Testing the Service/Repo Integration**

```ts
describe("User Service and Repository Integration", () => {
  beforeAll(async () => await setupTestDatabase());
  beforeEach(async () => await seedUser({ id: 1, user_name: "testuser" }));

  it("successfully fetches and formats name from real DB", async () => {
    const repo = new UserRepository();
    const service = new UserService(repo);
    const name = await service.getFormattedName(1);

    // Reveals the contract mismatch in the real query!
    expect(name).toBe("TESTUSER");
  });
});
```

## 🧠 **Real-World Case Study: "Before vs. After Integration Tests"**

- **Before (Unit Only):**
  > 500 unit tests pass. 5 minutes after deployment, logs flood with "null" errors because an auth module payload changed.
- **After (With Integration):**
  > 15 integration tests run in CI. One test for the "auth and fetch" flow fails, preventing the broken deployment.
- **The Improvement:** Integration tests act as a second layer of defense, catching boundary bugs that unit tests are blind to.

## 🤔 **Reflective Questions**

1. Why use a clean test DB instead of production?
2. How do you keep slow integration tests from bottlenecking the team?
3. What is the key difference between an integration test and an E2E test?

## 📚 **Further Reading**

- **Microsoft Docs:** Principles of integration testing (universal concepts).
- **Twelve-Factor App:** environment parity and robust app principles.

## 📝 **Mini Task (Production)**

Design an integration test for `checkWeatherAndNotify`:

- `WeatherService` fetches from a real/sandbox API.
- `NotificationService` sends an alert if it's freezing.

**Your Mission:**

1. Which real components would you involve?
2. What would you mock (if anything)?
3. What specific interaction would you verify?
