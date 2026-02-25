# 💡 CMT02.4: Praktik Mocking dengan Jest

**Outline:**

- **The "Smell" (SEEI):** The difficulty of testing a module when it directly `imports` and calls functions from another module.
- **The "Refactor" (PPP):** Using `jest.mock()` to automatically replace an entire module with a mocked version.
- **Your Mission (Production):** Write a test that verifies a logging function is called correctly using `jest.mock()`.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The "Smell" (SEEI)**

**The Code Smell: Hardcoded Dependencies**
When a file uses `import { doSomething } from './moduleB'`, it creates a testing problem. You can't easily inject a fake version of `doSomething`. The unit under test is tightly coupled to its concrete dependency, making true unit testing impossible.

**The Negative Impact: The Untestable Code**

- **Dependency Leaks:** Tests for `moduleA` accidentally run real code in `moduleB`, which might trigger API calls.
- **Inability to Test Error Paths:** How do you force `doSomething()` to throw an error to test `moduleA`'s error handling?
- **Complex Configuration:** You're forced into complex setups just to get dependencies into the correct state.

**Example of the Smell: Fetching User Data**

```ts
// api.ts
export async function fetchUser(userId: string) {
  const response = await axios.get(`https://api.example.com/users/${userId}`);
  return response.data;
}

// user-profile.ts
import { fetchUser } from "./api"; // Hardcoded dependency!
export async function getFormattedUsername(userId: string): Promise<string> {
  const user = await fetchUser(userId);
  return `${user.name.toUpperCase()} (#${user.id})`;
}
```

### **Part 2: Practice - The "Refactor" (PPP)**

**The Technique: Module Mocking with `jest.mock()`**
Jest can intercept `import`s and replace them with mock versions automatically.

1. **Call `jest.mock()`:** At the top level of your test file to replace the target module.
2. **Import the Mocked Function:** You get the mocked version instead of the original.
3. **Provide Fake Implementation:** Use `.mockResolvedValue()` or `.mockReturnValue()`.
4. **Assert:** Run code and verify the result and the mock's interaction.

**Step-by-Step Implementation: Testing `getFormattedUsername`**

```ts
// user-profile.test.ts
import { getFormattedUsername } from "./user-profile";
import { fetchUser } from "./api";

jest.mock("./api"); // Intercepts the import above
const mockedFetchUser = fetchUser as jest.Mock;

describe("getFormattedUsername", () => {
  it("formats username from fetched data", async () => {
    mockedFetchUser.mockResolvedValue({ id: "123", name: "John" });

    const result = await getFormattedUsername("123");
    expect(result).toBe("JOHN (#123)");
    expect(mockedFetchUser).toHaveBeenCalledWith("123");
  });
});
```

## 🧠 **Real-World Case Study: "Before vs. After Module Mocking"**

- **Before (Untestable):**
  > `analytics.ts` directly imports a `thirdPartySdk`. Testing logic pollutes production data.
- **After (Fully Testable):**
  > `jest.mock('thirdPartySdk')` is used. The team verifies `trackEvent` calls without triggering the SDK.
- **The Improvement:** Untestable code becomes isolated. Developers gain control to simulate any response or error.

## 🤔 **Reflective Questions**

1. Why must `jest.mock()` be at the top level?
2. `jest.mock()` vs. `jest.spyOn()`: When to use which?
3. How would you use `.mockRejectedValue()` for error paths?

## 📚 **Further Reading**

- **Jest Docs:** [Mock Functions](https://jestjs.io/docs/mock-function-api).
- **Kent C. Dodds:** Blogs on effective mocking practices.

## 📝 **Mini Task (Production)**

You have `logger.ts` and `app.ts`.

```ts
// logger.ts
export function logError(message: string, error?: Error) { ... }

// app.ts
import { logError } from './logger';
export function doRiskyOperation() {
    try { throw new Error('Bad!'); }
    catch (err) { logError('Failed', err as Error); }
}
```

**Your Mission:**
Write `app.test.ts`:

1. Mock `./logger`.
2. Call `doRiskyOperation`.
3. Assert `logError` was called exactly once with the correct arguments.
