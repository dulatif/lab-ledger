# 💡 CMT01.1: Mengapa Mengukur Kode?

**Outline:**

- **The "Smell" (SEEI):** The problem of subjective quality assessment ("This code feels complicated").
- **The "Refactor" (PPP):** Applying code metrics as an objective lens to analyze code.
- **Your Mission (Production):** Reflect on a past project and how metrics could have provided clarity.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The "Smell" (SEEI)**

**The Code Smell: Subjective Quality Assessment**
As architects and engineers, we often rely on "gut feeling." We look at a function and say, "This feels too complex," or "This code is a mess." While experience builds intuition, this approach is subjective, inconsistent, and leads to arguments based on opinion rather than fact. What one developer finds elegant, another might find obtuse, making it impossible to track improvements systematically.

**The Negative Impact: The "Argument from Authority"**

- **Inconsistent Standards:** Quality becomes a moving target that changes from developer to developer.
- **Blocks Progress:** Junior developers might hesitate to refactor "complex" code because they can't articulate _why_ it's a problem beyond "it's hard to read."
- **No Measurable Improvement:** You can't improve what you can't measure. Without data, you can't prove a refactoring actually reduced complexity.

**Example of the Smell: The Code Review Debate**

> **Dev A:** "This `processOrder` function is doing too much. We need to break it up."
> **Dev B:** "I don't think so. It's all related to order processing. It makes sense to keep it together."

This debate is based on opinion. Is the function objectively complex? We're just guessing.

### **Part 2: Practice - The "Refactor" (PPP)**

**The Technique: Introduce Code Metrics**
Code metrics are quantitative measures that provide an unbiased language to discuss quality. Instead of "this feels complex," you can say, "this has a Cyclomatic Complexity of 15, which is above our threshold of 10."

1. **Identify Key Metrics:** (e.g., Cyclomatic Complexity, Lines of Code, Code Coverage).
2. **Establish Baselines:** Measure your codebase to understand its current state.
3. **Set Thresholds:** Agree on reasonable thresholds as signals for closer investigation.

**Step-by-Step Implementation: Changing the Conversation**

> **Dev A:** "I ran a tool on `processOrder`. It has a Cyclomatic Complexity of 22. Our guidelines suggest investigating anything over 10. The high CC indicates many nested conditions, making it hard to test."
> **Dev B:** "Okay, that's a concrete problem. Let's look at the logical paths and see which parts we can extract."

## 🧠 **Real-World Case Study: "Before vs. After Analysis"**

- **Before (Subjective):**
  > "The old payment module is a nightmare. Nobody wants to touch it."
  - **Problem:** True but not actionable. Why is it a nightmare? Where are the worst parts?
- **After (Objective):**
  > "Analysis shows three functions (`calculateTaxes`, `applyPromotions`, `validateCard`) have complexities of 18, 25, and 30. These are our high-risk areas for refactoring."
  - **Improvement:** The vague complaint is now a focused, prioritized plan.

## 🤔 **Reflective Questions**

1. What are the dangers of treating metrics as unbreakable rules?
2. Besides complexity, what other aspects of code quality can be measured?
3. How can metrics help junior developers contribute to architecture discussions?

## 📚 **Further Reading**

- **Code Quality:** [Software quality on Wikipedia](https://en.wikipedia.org/wiki/Software_quality).
- **Metric Definitions:** Martin Fowler's blog on practical application of metrics.
- **Tools:** SonarQube's documentation on "Maintainability" ratings.

## 📝 **Mini Task (Production)**

Transform a subjective assessment from a past project into an objective observation.

1. Identify a piece of code described as "messy" or "confusing."
2. Determine which measurable attribute (nested `if`s, file length, parameters) contributed to that feeling.
3. Rewrite the assessment: e.g., "The `configure` function has 12 parameters and multiple nested `if` statements, indicating too many responsibilities."
