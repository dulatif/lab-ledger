# 💡 CMT01.2: Deep Dive: Cyclomatic Complexity

**Outline:**

- **The Smell (SEEI):** Understanding complex conditional logic and why it's a high-risk code smell.
- **The "Refactor" (PPP):** Applying Cyclomatic Complexity (CC) analysis to quantify logical complexity.
- **Your Refactoring Mission (Production):** Time to get your hands dirty and calculate the complexity of a function.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Smell (SEEI)**

**The Code Smell: Overly Complex Logic**
This smell exists in functions filled with decision points: `if`, `else`, `while`, `for`, `switch`, `&&`, `||`, and ternaries. Every new path increases the scenarios you must test and hold in your head. This is where bugs hide. A function with many paths is inherently harder to reason about, test, and modify safely.

**The Negative Impact: The Untestable Function**
High logical complexity directly translates to project risk:

- **High Bug Density:** Complexity strongly correlates with the number of defects.
- **Difficult to Test:** To be confident, you need one test case for every possible path. This quickly becomes impractical.
- **Hard to Maintain:** Changing a complex function is like surgery in the dark; it's difficult to predict side effects on other paths.

**Example of the Smell: The `getDiscount` function**
Full of branching logic despite looking simple.

```ts
function getDiscount(user, product) {
  let discount = 0;
  if (user.isPremium) {
    discount += 0.1;
  }
  if (product.category === "books" && user.booksPurchased > 5) {
    discount += 0.05;
  }
  if (new Date().getMonth() === 11) {
    // December promotion
    discount += 0.05;
  }
  return discount > 0.2 ? 0.2 : discount; // Cap the discount
}
```

How many paths are there? How many tests do you need?

### **Part 2: Practice - The "Refactor" (PPP)**

**The Technique: Cyclomatic Complexity (CC) Analysis**
CC measures the number of decisions in code.

- **Rule of Thumb:**
  - **1-5:** Simple, low risk.
  - **6-10:** Moderately complex.
  - **11-20:** High complexity, hard to test.
  - **>20:** Very high risk, needs immediate attention.

- **How to Calculate:**
  1. Start with a score of **1**.
  2. Add **1** for every `if`, `while`, `for`, `case`, `catch`.
  3. Add **1** for every logical AND (`&&`) and OR (`||`).
  4. Add **1** for every ternary operator (`? :`).

**Step-by-Step Implementation: Calculating CC for `getDiscount`**

```ts
function getDiscount(user, product) {
  // Start with 1
  let discount = 0;
  if (user.isPremium) {
    // +1 (Total: 2)
    discount += 0.1;
  }
  if (product.category === "books" && user.booksPurchased > 5) {
    // +1 if, +1 && (Total: 4)
    discount += 0.05;
  }
  if (new Date().getMonth() === 11) {
    // +1 (Total: 5)
    discount += 0.05;
  }
  return discount > 0.2 ? 0.2 : discount; // +1 ternary (Total: 6)
}
```

CC is **6**. You need at least 6 test cases.

## 🧠 **Real-World Case Study: "Before vs. After Analysis"**

- **Before (High CC: 15):**
  A single `processInput(input)` with many nested statements.
- **After (Distributed Complexity):**
  ```ts
  function processInput(input) {
    if (!isValid(input)) return handleError();
    const data = parse(input);
    return processData(data);
  }
  ```
- **The Improvement:** Total complexity is distributed into manageable, independently testable functions (`isValid`, `parse`, `processData`). `processInput` is now just a summary (CC: 2).

## 🤔 **Reflective Questions**

1. What does a function with CC of 1 look like?
2. If two functions have the same CC but different lengths, which is more maintainable?
3. How does aiming for low CC guide you toward SRP?

## 📚 **Further Reading**

- **Original Paper:** Thomas J. McCabe (1976), "A Complexity Measure."
- **Smells:** [Long Method](https://refactoring.guru/smells/long-method) on refactoring.guru.
- **Techniques:** [Decompose Conditional](https://refactoring.guru/refactorings/decompose-conditional).

## 📝 **Mini Task (Production)**

Calculate Cyclomatic Complexity for `validateUser`. Show your work line-by-line.

```ts
function validateUser(user: User | null): string {
  if (!user || !user.profile) {
    return "Invalid user object.";
  }

  if (user.profile.age < 18 && !user.hasParentalConsent) {
    return "User is underage and lacks consent.";
  }

  let status = "Pending";
  for (const doc of user.documents) {
    if (doc.status === "verified") {
      status = "Verified";
      break;
    }
  }

  return status === "Verified"
    ? "User is verified."
    : "User needs to submit documents.";
}
```
