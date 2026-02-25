# 💡 BRT03.1: Decompose Conditional

**Outline:**

- **The Smell (SEEI):** Identifying and understanding the "Complex Conditional" block.
- **The Refactor (PPP):** Applying the "Decompose Conditional" technique by extracting logic into methods.
- **Your Refactoring Mission (Production):** Time to break down a complex pricing logic.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Smell (SEEI)**

**The Code Smell: Complex Conditional**
This occurs when you have a large `if-then-else` structure where the branches contain significant, complex logic. The high-level flow (the `if` check) is mixed with low-level implementation details (the work inside the branches), making the function hard to read.

**The Negative Impact**

- **Hides Intent:** Clutter makes it hard to see the overall story of the function.
- **Low Cohesion:** The `if` branch and `else` branch often do entirely different things, yet they are forced together.
- **Difficult to Test:** You can't easily test the branches independently without satisfying the main conditional.
- **Hard to Reuse:** Branch logic is "trapped" inside the conditional, preventing reuse without duplication.

**Example of the Smell**
A subscription calculator with complex seasonal pricing dumped inside the `if/else`.

```ts
function calculateCharge(date: Date, plan: Plan, quantity: number): number {
  if (date >= summerStart && date <= summerEnd) {
    // Complex summer pricing...
    return quantity * plan.summerServiceCharge * 0.8;
  } else {
    // Complex regular pricing...
    return quantity * plan.regularRate + plan.regularServiceCharge;
  }
}
```

### **Part 2: Practice - The Refactor (PPP)**

**The Technique: Decompose Conditional**
The goal is to separate flow control from implementation details using "Extract Method."

1. **Extract the `if` block** logic into a well-named method (e.g., `calculateSummerCharge`).
2. **Extract the `else` block** logic into another method (e.g., `calculateRegularCharge`).
3. **Extract the condition** check into its own function (e.g., `isSummer(date)`).

**Step-by-Step Implementation**

1. Extract summer math -> `calculateSummerCharge`.
2. Extract regular math -> `calculateRegularCharge`.
3. Extract date range check -> `isSummer`.

## 🧠 **Real-World Case Study: "Before vs. After Refactoring"**

- **Before (Complex):**
  A developer reading `calculateCharge` has to process date math and multiplication factors simultaneously.
- **After (Decomposed):**
  ```ts
  function calculateCharge(date: Date, plan: Plan, quantity: number): number {
    return isSummer(date)
      ? calculateSummerCharge(plan, quantity)
      : calculateRegularCharge(plan, quantity);
  }
  ```
- **The Improvement:** The function now reads like a table of contents. Each branch is a clean, single-responsibility function that can be tested independently.

## 🤔 **Reflective Questions**

1. How does Decompose Conditional improve code testability?
2. When does an `if` block become "too complex" for a single function?
3. How do you find the right balance between too many small functions and one large conditional?

## 📚 **Further Reading**

- **The Bible:** Martin Fowler's "Refactoring".
- **Refactoring.Guru:** [Decompose Conditional](https://refactoring.guru/decompose-conditional).

## 📝 **Mini Task (Production)**

Refactor this `processEndOfYear` function by decomposing the full-time vs. part-time logic.

```ts
function processEndOfYear(employee: Employee): void {
  if (employee.type === "FULL_TIME") {
    // 1. Extract this to handleFullTime(employee)
    employee.vacationDays += 5;
    if (!employee.benefits.includes("PENSION"))
      employee.benefits.push("PENSION");
  } else {
    // 2. Extract this to handlePartTime(employee)
    const pay = (employee.hourlyRate || 0) * (employee.hoursWorked || 0);
    if ((employee.hoursWorked || 0) > 1000) employee.vacationDays += 2;
  }
}
```

**Mission:** Create the helper functions and simplify the main `processEndOfYear` method.
