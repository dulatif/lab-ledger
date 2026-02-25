# 💡 BRT02.3: Introduce Explaining Variable

**Outline:**

- **The Smell (SEEI):** Identifying and understanding "Complex Expressions" and "Magic Values."
- **The Refactor (PPP):** Applying the "Introduce Explaining Variable" technique.
- **Your Refactoring Mission (Production):** Time to untangle a complex condition.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Smell (SEEI)**

**The Code Smell: Complex Expression (and Magic Values)**
This smell appears when logic—inside an `if` statement or a calculation—is hard to understand at a glance. It often bundles multiple boolean checks or "magic" literal values whose purpose is hidden. The code works, but the "Why" is opaque.

**The Negative Impact**

- **Hides Intent:** The expression tells you _what_ it's doing, not _why_. `if (isEligibleForDiscount)` is a business rule; the raw check is just math.
- **Hard to Debug:** When a complex expression fails, you can't easily see which part of the condition was the culprit.
- **Encourages Duplication:** Cryptic lines are often copied and pasted because they're too scary to refactor.
- **Requires Comments:** When code needs a comment to explain a logic block, the code itself is failing to communicate.

**Example of the Smell**
Can you understand this in 5 seconds?

```ts
if (
  system.platform.toUpperCase().indexOf("MAC") > -1 &&
  system.browser.toUpperCase().indexOf("IE") > -1 &&
  system.isResized
) {
  // ...
}
```

### **Part 2: Practice - The Refactor (PPP)**

**The Technique: Introduce Explaining Variable**

1. **Identify a sub-part** of the complex expression.
2. **Extract it** into a well-named `const` variable.
3. **Replace the sub-expression** with the new variable.
4. **Repeat** until the original logic is readable prose.

**Step-by-Step Implementation**

1. **Extract Platform:** `const isMac = ...`
2. **Extract Browser:** `const isIE = ...`
3. **Compound Rule:** `const isPeculiarMacOSCase = isMac && isIE;`
4. **Final Logic:** `if (isPeculiarMacOSCase && system.isResized) ...`

## 🧠 **Real-World Case Study: "Before vs. After Refactoring"**

- **Before (Complex):**
  Deeply nested string checks and boolean operations. Hard to log or trace.
- **After (Explaining Variables):**

  ```ts
  const isMac = platform.includes('MAC');
  const isIE = browser.includes('IE');
  const isPeculiarMacOSCase = isMac && isIE;

  if (isPeculiarMacOSCase && system.isResized) { ... }
  ```

- **The Improvement:** Separation of "How" (string checks) from "What" (the business rule). New developers immediately grasp the rule, and debugging individual flags is straightforward.

## 🤔 **Reflective Questions**

1. How does this technique make code easier to debug?
2. When should you use "Extract Method" instead of "Explain Variable"?
3. Can you over-use this? What's the downside of extracting _every_ single comparison?

## 📚 **Further Reading**

- **The Bible:** Martin Fowler's "Refactoring".
- **Refactoring.Guru:** [Introduce Explaining Variable](https://refactoring.guru/introduce-explaining-variable).
- **Tools:** Use VS Code's "Extract to constant" (`Ctrl + .`).

## 📝 **Mini Task (Production)**

Refactor this complex `return` statement in a ticket pricer.

```ts
function getTicketPrice(movie: Movie, customer: Customer, day: string): number {
  return (
    (day === "WEEKEND" ? 15 : 12) +
    (movie.type === "IMAX" ? 5 : movie.type === "3D" ? 3 : 0) -
    (customer.age < 12 || customer.age > 65 || customer.isStudent ? 2 : 0)
  );
}
```

**Variables to extract:** `basePrice`, `movieSurcharge`, `isEligibleForDiscount`, `discountAmount`.
