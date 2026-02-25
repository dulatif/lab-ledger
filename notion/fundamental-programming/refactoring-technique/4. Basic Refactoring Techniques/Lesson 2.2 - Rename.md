# 💡 BRT02.2: Rename

**Outline:**

- **The Smell (SEEI):** Understanding the high risk of "Manual Renaming."
- **The Refactor (PPP):** Using the automated "Rename Symbol" feature in your IDE.
- **Your Refactoring Mission (Production):** Time to perform a safe, large-scale rename.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Smell (SEEI)**

**The Code Smell: Manual Renaming & Inconsistent Naming**
This is a process smell. It happens when you change the name of a variable, function, or class used in multiple places by hand, often using "Find and Replace." This method is dangerously unaware of code syntax and context, leading to inconsistent naming (e.g., mixing `User` and `Customer` for the same entity).

**The Negative Impact**

- **Breaks Code:** Global "Find and Replace" for `id` might accidentally break a word like `grid` into `gr-id`. It understands strings, not syntax.
- **Incomplete:** It's easy to miss a reference in another file, causing compilation errors or runtime bugs.
- **Increases Confusion:** Renaming 3 out of 4 instances of `user` to `accountHolder` makes the code _more_ inconsistent.
- **Erodes Trust:** Fear of breakage prevents developers from improving names, leading to technical debt.

**Example of the Smell (The Flawed Process)**
Renaming `getData()` (used in 10 files) to `fetchUserData()` manually:

1. Global search for `getData(`.
2. Sifting through 20 results across 10 files.
3. Manually changing each call and definition.
4. Praying you didn't miss a reference.

### **Part 2: Practice - The Refactor (PPP)**

**The Technique: Automated Rename Symbol (F2)**
Your IDE holds a semantic understanding of your code. It knows that a function call in one file points to a definition in another. The "Rename Symbol" feature leverages this for safe, atomic operations.

In VS Code: **`F2`**.

**Step-by-Step Implementation**

1. **Place Cursor:** Click on any usage or definition of the symbol.
2. **Press `F2`:** A small input box appears.
3. **Type Name:** Type the new name and press `Enter`.

**Result:** The IDE atomically updates the definition, all imports, and all usages across the entire project.

## 🧠 **Real-World Case Study: "Before vs. After Refactoring"**

- **Before (Risky Manual Rename):**
  Using "Find and Replace" to change class `Record` to `UserRecord`.
  `const audit = "A new Record was created.";` -> Broken into `"A new UserRecord..."`, potentially breaking log parsers.
- **After (Safe Automated Rename):**
  The IDE knows a string is just a string. It renames the class and types, but leaves the `auditMessage` string content untouched.
- **The Improvement:** Automated renaming removes fear. It transforms a dangerous manual task into a trivial, everyday action, allowing constant improvement of code clarity.

## 🤔 **Reflective Questions**

1. Why is "Rename" often called the most important refactoring to learn?
2. What's the difference between textual "Find and Replace" and symbolic "Rename"?
3. When might an automated rename still be insufficient (e.g., config files)?

## 📚 **Further Reading**

- **The Bible:** Martin Fowler's "Refactoring" (Rename is a fundamental move).
- **VS Code Docs:** [Rename Symbol](https://code.visualstudio.com/docs/editor/refactoring#_rename-symbol).
- **Blog:** "You can't have clean code without this one skill."

## 📝 **Mini Task (Production)**

Use **only** `F2` to perform these renames across three files:

1. Rename `doCalc` function to `sumValues`.
2. Rename `data` variable to `inventoryItems`.
3. Rename the `val` property on the `Thing` interface to `value`.

Notice how changing a property in one file automatically fixes its property access throughout the project.
