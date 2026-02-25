# 💡 CMT02.2: Tes untuk Kode Legacy

**Outline:**

- **The "Smell" (SEEI):** An important piece of legacy code that has no tests and its behavior is not fully understood.
- **The "Refactor" (PPP):** Applying "Characterization Tests" to lock down the code's current behavior _as-is_ before refactoring.
- **Your Mission (Production):** Write a characterization test for a convoluted legacy function.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The "Smell" (SEEI)**

**The Code Smell: The Unknown Behavior of Legacy Code**
You need to change complex, important code, but it has zero tests. You can't refactor safely without a safety net. Worse, you aren't 100% sure what it actually does. Existing bugs might even be relied upon by other systems. Writing "correct" unit tests is impossible because you don't know what "correct" is yet.

**The Negative Impact: Refactoring Paralysis**

- **Blind Guesses:** Any change might fix one thing but break five others.
- **Risky "Intended" Tests:** Writing tests for what you _think_ it should do might break actual dependencies.
- **System Rot:** The module becomes "untouchable," leading to ugly workarounds elsewhere.

**Example of the Smell: The `generateLegacyId` function**

```ts
// Legacy code. No one knows why it does this, but other systems rely on it.
function generateLegacyId(name: string, year: number): string {
  if (!name) return "INVALID";
  const cleanedName = name.trim().toUpperCase().substring(0, 4);
  const yearCode = (year - 1980) % 50; // Why 1980? Why % 50?
  return `${cleanedName}_${yearCode}`;
}
```

### **Part 2: Practice - The "Refactor" (PPP)**

**The Technique: Characterization Tests (The Golden Master)**
A characterization test documents the **actual, current behavior**, not the intended behavior. The goal is to prevent unintended changes during refactoring.

1. **Write a Test Harness:** Create a test calling the legacy code with specific inputs.
2. **Execute and Observe:** Run the code and record the raw output.
3. **Assert Observed Behavior:** Write an assertion to lock in the result. You are characterizing what it _does_, not what it _should_ do.
4. **Repeat:** Cover a wide range of inputs to map the behavior.

**Step-by-Step Implementation: Characterizing `generateLegacyId`**

1. **Observe:** `generateLegacyId('Example', 2023)` returns `EXAM_43`.
2. **Lock in:**

```ts
it("characterize normal name and year", () => {
  const result = generateLegacyId("Example", 2023);
  expect(result).toBe("EXAM_43"); // Lock in current behavior
});

it("characterize null input", () => {
  expect(generateLegacyId(null, 2023)).toBe("INVALID");
});
```

## 🧠 **Real-World Case Study: "Before vs. After Refactoring"**

- **Before (Paralysis):**
  > Critical `TransactionProcessor` needs an update for a new gateway. Developers fear touching it and breaking old gateways.
- **After (Safety Net):**
  > The team writes comprehensive characterization tests using dozens of real-world inputs.
- **The Improvement:** The tests act as a "change detector." The team refactors with confidence, knowing any accidental change to old behavior will be caught immediately.

## 🤔 **Reflective Questions**

1. Why shouldn't you "fix" bugs discovered during characterization testing?
2. How is this different from a TDD-style unit test?
3. What happens to these tests after the refactoring is complete?

## 📚 **Further Reading**

- **Michael Feathers:** "Working Effectively with Legacy Code."
- **Martin Fowler:** Golden Master technique articles.

## 📝 **Mini Task (Production)**

Write characterization tests for `getStatusText` without changing the logic.

```ts
function getStatusText(user: any): string {
  if (user.is_admin) return "ADMIN";
  if (user.deactivated == true) return "Inactive";
  if (user.posts > 100 && user.membership_level >= 3) return "Power User";
  return user.posts > 10 ? "Regular" : "Newbie";
}
```

**Your Mission:**
Lock in current behavior for:

1. Admin: `{ is_admin: true, deactivated: false, posts: 5, membership_level: 1 }`
2. Deactivated: `{ is_admin: false, deactivated: true, posts: 500, membership_level: 5 }`
3. Power User: `{ posts: 101, membership_level: 3 }`
4. Regular: `{ posts: 11 }`
5. Newbie: `{ posts: 9 }`
