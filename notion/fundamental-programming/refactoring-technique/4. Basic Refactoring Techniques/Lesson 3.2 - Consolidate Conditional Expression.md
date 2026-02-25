# 💡 BRT03.2: Consolidate Conditional Expression

**Outline:**

- **The Smell (SEEI):** Identifying multiple, separate conditional checks that lead to the same result.
- **The Refactor (PPP):** Applying the "Consolidate Conditional Expression" technique using logical operators.
- **Your Refactoring Mission (Production):** Time to combine a series of eligibility checks.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Smell (SEEI)**

**The Code Smell: Fragmented Conditionals**
This occurs when a sequence of separate checks (`if`, `else if`) all result in the same outcome. While technically correct, it presents the logic as a series of separate ideas rather than a single, unified business rule.

**The Negative Impact**

- **Hides Intent:** It's not obvious that separate checks are part of one larger concept.
- **Encourages Duplication:** Adding another check often means adding another redundant `if` block.
- **Harder to Maintain:** If the outcome (e.g., `return 0`) needs to change, you must update it in multiple places.
- **Blocks Refactoring:** You can't easily use "Extract Method" on fragmented code.

**Example of the Smell**
Checks for disability benefits spread across multiple `if` statements.

```ts
function disabilityAmount(person: Person): number {
  if (person.seniority < 2) return 0;
  if (person.monthsDisabled > 12) return 0;
  if (person.isPartTime) return 0;

  return 1000;
}
```

### **Part 2: Practice - The Refactor (PPP)**

**The Technique: Consolidate Conditional Expression**
Combine separate checks into a single expression using logical operators (`&&`, `||`).

1. **Identify** conditionals producing the same result.
2. **Merge** them into a single `if` statement.
3. **Extract (Recommended):** Move the unified condition into its own well-named function.

**Step-by-Step Implementation**

1. Combine with OR: `if (seniority < 2 || monthsDisabled > 12 || isPartTime)`.
2. Extract to method: `if (isNotEligibleForDisability(person))`.

## 🧠 **Real-World Case Study: "Before vs. After Refactoring"**

- **Before (Fragmented):**
  A sequence of `if` statements that feel like separate rules, making the logic look more complex than it is.
- **After (Consolidated):**
  ```ts
  function disabilityAmount(person: Person): number {
    if (isNotEligibleForDisability(person)) return 0;
    return 1000;
  }
  ```
- **The Improvement:** The business rule is now an explicit, single unit. The code communicates the high-level intent first, hiding the details of the eligibility logic in a clean helper function.

## 🤔 **Reflective Questions**

1. What's the difference between consolidating conditions with the same result vs. different results?
2. How does consolidation enable "Extract Method"?
3. When is keeping checks separate actually _more_ readable?

## 📚 **Further Reading**

- **The Bible:** Martin Fowler's "Refactoring".
- **Refactoring.Guru:** [Consolidate Conditional Expression](https://refactoring.guru/consolidate-conditional-expression).

## 📝 **Mini Task (Production)**

Refactor this `getDiscount` function by consolidating the fragmented level and spend checks.

```ts
function getDiscount(user: UserProfile): number {
  let discount = 0;

  // Consolidate these!
  if (user.level === "GOLD") discount = 0.15;
  const totalSpent = user.purchaseHistory.reduce((sum, item) => sum + item, 0);
  if (totalSpent > 1000) discount = 0.15;

  // This is a guard clause (different logic), keep it separate
  if (user.lastLogin < oneYearAgo) return 0;

  return discount;
}
```

**Mission:** Create an `isEligibleForLoyaltyDiscount` function and use it to simplify the pricing logic.
