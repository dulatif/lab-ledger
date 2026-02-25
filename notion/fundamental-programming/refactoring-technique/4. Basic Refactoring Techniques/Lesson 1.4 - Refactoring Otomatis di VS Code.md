# 💡 BRT01.4: Refactoring Otomatis di VS Code

**Outline:**

- **The Smell (SEEI):** Understanding the risks and inefficiency of "Manual Refactoring".
- **The Refactor (PPP):** Using the built-in automated refactoring tools in Visual Studio Code.
- **Your Refactoring Mission (Production):** Time to let your tools do the heavy lifting.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Smell (SEEI)**

**The Code Smell: Manual Refactoring**
This is a smell in your _process_. Manual Refactoring is the act of structural change by hand: copying, pasting, deleting, and manually renaming across multiple files.

**The Negative Impact**

- **Slow:** Takes significant time and mental effort that could be used for solving business problems.
- **Error-Prone:** Easy to miss a usage, forget a parameter, or make a copy-paste error, leading to hard-to-track bugs.
- **Discourages Good Habits:** Because it's tedious and risky, developers avoid small, frequent cleanups, leading to massive technical debt.

**Example of the Smell (The Process)**
To extract a discount logic manually:

1. Create `function applyDiscount()`.
2. Copy the `if` block inside.
3. Identify and add parameters (`basePrice`, `isPremium`).
4. Add return type.
5. Fix logic to `return` values.
6. Replace the original block with a call.
7. Reread to ensure no mistakes.
   _This is exhausting just to describe._

### **Part 2: Practice - The Refactor (PPP)**

**The Technique: Automated Refactoring in VS Code**
Modern IDEs like VS Code have powerful, language-aware tools to perform these refactorings instantly and safely.

The primary shortcut: **`Ctrl + .`** (Quick Fix / Refactor).

**Step-by-Step Implementation**

1. **Select the Code:** Highlight the `if` block.
2. **Open the Menu:** Press `Ctrl + .`.
3. **Choose the Action:** Select "Extract to function in global scope" (or similar).
4. **Name it:** Type `applyDiscount`.

**Done.** In seconds, VS Code handles parameters, return types, and call replacement safely.

**Other Tools:**

- **Rename:** Press `F2` on a variable/function name to rename it and all its references accurately.
- **Inline:** Use `Ctrl + .` on a function call to "Inline Method".

## 🧠 **Real-World Case Study: "Manual vs. Automated"**

- **The Task:** Rename a private helper used in 3 public methods.
- **Manual Approach:**
  - Time: 1-2 minutes.
  - Risk: High (typos, missed references).
- **Automated Approach (`F2`):**
  - Time: 5 seconds.
  - Risk: Near zero.
- **The Improvement:** Automated tools turn refactoring from a risky chore into a quick, safe, everyday activity, encouraging continuous improvement (The Boy Scout Rule).

## 🤔 **Reflective Questions**

1. What are the limitations of automated refactoring tools?
2. How does relying on these tools change your willingness to clean up code?
3. What other automated refactorings have you found in your IDE?

## 📚 **Further Reading**

- **VS Code Docs:** [Refactoring Documentation](https://code.visualstudio.com/docs/editor/refactoring).
- **Video:** "TypeScript Refactoring in VS Code" on YouTube.
- **Principle:** The Boy Scout Rule.

## 📝 **Mini Task (Production)**

Use **only** automated tools (no manual typing/copying) for these tasks in a `payroll.ts` file:

1. **Task 1:** Select the line `pay = hours * rate` and use **"Extract to constant"** (`basePay`).
2. **Task 2:** Select the `if (isManager)` block and use **"Extract to function"** (`calculateBonus`).
3. **Task 3:** Use **"Inline method"** on a `calculatePayroll` call.
