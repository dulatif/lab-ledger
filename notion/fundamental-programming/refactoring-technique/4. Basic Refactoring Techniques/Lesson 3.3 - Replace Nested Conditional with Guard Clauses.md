# 💡 BRT03.3: Guard Clauses

**Outline:**

- **The Smell (SEEI):** Identifying the "Arrowhead Code" of deeply nested conditionals.
- **The Refactor (PPP):** Applying "Guard Clauses" to flatten the code and clarify the main path.
- **Your Refactoring Mission (Production):** Time to slay an arrowhead.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Smell (SEEI)**

**The Code Smell: Nested Conditional (Arrowhead Code)**
This infamous smell occurs when `if-else` statements are nested inside other `if-else` statements, causing the code to drift rightward. This "arrowhead" shape is hard to read because it forces you to maintain a mental stack of multiple true/false conditions simultaneously.

**The Negative Impact**

- **High Cognitive Load:** To understand the 4th level of nesting, you must remember the outcomes of the 3 preceding checks.
- **Hides the "Happy Path":** The main execution path is buried deep inside, while outer layers focus on exceptional cases and errors.
- **Error-Prone:** It's easy to place logic in the wrong `else` block or miss a required check.
- **Difficult to Modify:** Adding one more condition often means adding yet another level of indentation.

**Example of the Smell**
A payroll function where the actual normal pay calculation is buried at the 3rd level of nesting.

```ts
function getPayAmount(employee: Employee): number {
  if (employee.isSeparated) {
    return 0;
  } else {
    if (employee.isRetired) {
      return 1000;
    } else {
      // HAPPY PATH IS BURIED HERE
      return 5000;
    }
  }
}
```

### **Part 2: Practice - The Refactor (PPP)**

**The Technique: Replace Nested Conditional with Guard Clauses**
A "Guard Clause" is a check at the top of a function that exits immediately if a precondition isn't met. This "fails fast" and clears the way for the happy path.

1. **Identify** the condition leading to an early exit.
2. **Invert** and return immediately at the top (the "Guard").
3. **Promote** the remaining logic one level of indentation left.
4. **Repeat** until the code is flat.

**Step-by-Step Implementation**

1. Guard for `isSeparated`: `if (isSeparated) return 0;`
2. Guard for `isRetired`: `if (isRetired) return 1000;`
3. Final path: `return 5000;` (at top level).

## 🧠 **Real-World Case Study: "Before vs. After Refactoring"**

- **Before (Arrowhead):**
  A developer has to navigate several `else` blocks to find where the actual work happens. The structure emphasizes exceptions over the main goal.
- **After (Guard Clauses):**
  ```ts
  function getPayAmount(employee: Employee): number {
    if (employee.isSeparated) return 0;
    if (employee.isRetired) return 1000;

    return calculateNormalPay(employee);
  }
  ```
- **The Improvement:** Flat, readable code. Special cases are handled and dismissed immediately, leaving the core logic obvious and un-indented.

## 🤔 **Reflective Questions**

1. How do Guard Clauses relate to the concept of "fail fast"?
2. Why is "returning early" often better than a "single exit point"?
3. Can a function have _too many_ guard clauses?

## 📚 **Further Reading**

- **The Bible:** Martin Fowler's "Refactoring".
- **Refactoring.Guru:** [Guard Clauses](https://refactoring.guru/replace-nested-conditional-with-guard-clauses).
- **Pattern:** The "Return Early" pattern.

## 📝 **Mini Task (Production)**

Refactor this deeply nested access controller using Guard Clauses.

```ts
function getAccess(user: User, doc: Document): string {
  if (!user.isLoggedIn) return doc.isPublic ? "READ" : "NONE";
  if (!user.isVerified) return "NONE";
  if (user.role === "ADMIN") return "WRITE";
  if (doc.authorId === user.id) return "WRITE";

  return doc.isPublic ? "READ" : "NONE";
}
```

**Mission:** Convert the `if-else` mess into a clean sequence of checks that prioritize early exits for errors and special roles.
