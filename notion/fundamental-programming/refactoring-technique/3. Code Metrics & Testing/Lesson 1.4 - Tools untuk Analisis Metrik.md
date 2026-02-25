# 💡 CMT01.4: Tools untuk Analisis Metrik

**Outline:**

- **The Smell (SEEI):** The inefficiency and inconsistency of analyzing code quality manually.
- **The "Refactor" (PPP):** Integrating automated metric analysis tools directly into the development workflow.
- **Your Refactoring Mission (Production):** Install and use a VS Code extension to analyze a piece of code.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Smell (SEEI)**

**The Code Smell: Manual Quality Analysis**
Manually calculating complexity or checking coverage for every function is slow, tedious, and error-prone. If quality analysis is a difficult, separate step done only occasionally, it won't be effective. Checks must be fast, automated, and integrated into your daily workflow to provide immediate feedback while you write code.

**The Negative Impact: Quality as an Afterthought**

- **Inconsistent Application:** Developers skip manual checks under tight deadlines.
- **Slow Feedback Loop:** You might not realize a function is overly complex until weeks later during a review.
- **Doesn't Scale:** Manual assessment of thousands of files is impossible; you can't spot systemic risks.

**Example of the Smell: The "End-of-Sprint" Review**

> **Team Lead:** "Before we deploy, let's spend the afternoon manually reviewing everyone's code for complexity."
> This is too little, too late. Feedback should happen the moment complex code is written.

### **Part 2: Practice - The "Refactor" (PPP)**

**The Technique: Integrate Automated Tooling**
Let machines do the work at two stages:

1. **In the IDE:** Real-time feedback as you type (the fastest loop).
2. **In Continuous Integration (CI):** Quality gates for the team on every commit/PR.

**Step-by-Step Implementation: Real-Time Feedback in VS Code**
Focus on getting instant feedback in your editor using the `CodeMetrics` extension.

1. **Open VS Code.**
2. **Extensions View:** (Ctrl+Shift+X).
3. **Search:** `CodeMetrics` by `kisstkondoros`.
4. **Install.**
5. **Analyze:** Open a TS file; gray annotations appear above functions showing their complexity.
6. **Customize (Optional):** Update `settings.json` to color-code high complexity (e.g., CC > 10 turns red).

## 🧠 **Real-World Case Study: "Before vs. After Workflow"**

- **Before (Manual/CI Only):**
  > Developer writes a function (CC: 18). An hour later, CI fails. The developer must context-switch back to refactor.
- **After (IDE Integration):**
  > As the developer adds an `if` statement, the metric turns yellow (CC: 11). They pause and refactor immediately while the logic is fresh.
- **The Improvement:** Feedback loop reduced from hours to seconds. Quality debt is prevented from accumulating.

## 🤔 **Reflective Questions**

1. Pros and cons of CI quality gates vs. IDE info?
2. How can ESLint enforce complexity limits automatically?
3. What other metrics can tools like SonarQube track?

## 📚 **Further Reading**

- **Marketplace:** Search for `SonarLint` or `ESLint`.
- **SonarQube:** Explore [sonarqube.org](https://www.sonarqube.org/) for team-wide management.
- **ESLint:** Check the [complexity rule](https://eslint.org/docs/latest/rules/complexity) documentation.

## 📝 **Mini Task (Production)**

Install and use an analysis tool.

1. Install `CodeMetrics` in VS Code.
2. Paste the `validateUser` function (from Lesson 1.2) into a TS file.
3. Observe the annotation.
4. **Report findings:** What score does the tool report? Does it match your manual calculation?
