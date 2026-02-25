# 💡 CC03.4: Automating Cleanliness with Linters and Formatters

**Outline:**

- **The Smell (SEEI):** Identifying "Inconsistent Style" and manually catching trivial errors.
- **The "Refactor" (PPP):** Applying automated tools—ESLint (a linter) and Prettier (a formatter)—to enforce consistency.
- **Your Refactoring Mission (Production):** Time to set up a basic configuration for these tools.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Smell (SEEI)**

**The Code Smell: Inconsistent Style and "Bikeshedding"**
This smell occurs at the team level. One developer uses tabs, another uses spaces. This creates a visually jarring and inconsistent codebase. The related process smell is **"bikeshedding"**—teams wasting hours in code reviews arguing about trivial style choices instead of focusing on actual logic.

**The Negative Impact: Why does inconsistency matter?**

- **Wasted Time:** Arguing about style is a drain on productivity.
- **Increased Cognitive Load:** Your brain has to adjust to different styles in different files.
- **Useless Diffs:** Reformatting files creates "noise" in `git diff`, hiding the actual logic changes.
- **Allows Simple Bugs:** Simple mistakes, like unused variables, can be missed by the human eye but are easily caught by machines.

**Example of the Smell: Two different styles in one project**

```ts
// File 1: developer_anna.ts
function sayHello(name: string): string {
  const message = "Hello, " + name;
  return message;
}

// File 2: developer_bob.ts
function sayGoodbye(name: string): string {
  const message = `Goodbye, ${name}`;
  return message;
}
```

This code works, but the style is inconsistent (spacing, quotes, semicolons).

### **Part 2: Practice - The "Refactor" (PPP)**

**The Technique: Automate Everything with ESLint and Prettier**
Agree on rules once, configure the tools, and let them enforce consistency automatically.

1. **ESLint (The Linter):** Analyzes code to find potential _problems_ (unused variables, best practices).
2. **Prettier (The Formatter):** Concerned only with _style_ (indentation, quotes, line wrapping).

**Step-by-Step Implementation: Setting up a Basic Configuration**
In a typical TypeScript project:
`npm install --save-dev eslint prettier @typescript-eslint/parser @typescript-eslint/eslint-plugin eslint-config-prettier`

1. **Configure ESLint (`.eslintrc.json`):**

```json
{
  "parser": "@typescript-eslint/parser",
  "extends": ["plugin:@typescript-eslint/recommended", "prettier"],
  "root": true
}
```

2. **Configure Prettier (`.prettierrc.json`):**

```json
{
  "semi": true,
  "singleQuote": true,
  "trailingComma": "es5",
  "printWidth": 80
}
```

### **Run the Tools:**

Add scripts to `package.json` or configure your IDE to run them automatically on save.

## 🧠 **Real-World Case Study: "Before vs. After Automation"**

- **Before (Manual Formatting):**
  > Code Review Comment: "Nit: Can you please use single quotes here?"
  > Result: Developer has to push again, wasting time and CI resources.
- **After (Automated Formatting):**
  > Developer saves the file; Prettier fixes it instantly. The team never discusses quotes again. ESLint catches an unused variable before the code is even committed.
- **The Improvement:**
  - **Consistency:** The entire codebase looks like it was written by one person.
  - **Efficiency:** Zero time wasted on style arguments.
  - **Higher Quality:** Linters act as a first-pass automated code review.
  - **Better Code Reviews:** Focus shifts to logic and architecture.

## 🤔 **Reflective Questions**

1. What is the key difference between a linter and a formatter?
2. How does integrating these tools into CI/CD help maintain code quality?
3. What are the arguments for and against letting a tool make all of your style decisions?

## 📚 **Further Reading**

- **ESLint:** [Official Documentation](https://eslint.org/)
- **Prettier:** [Official Documentation](https://prettier.io/)
- **VS Code Integration:** [Using ESLint and Prettier in TypeScript](https://www.robinwieruch.de/vscode-eslint/)

## 📝 **Mini Task (Production)**

You are given a messy TypeScript file.

**Your Mission:**

1. Write a basic `.eslintrc.json`.
2. Write a basic `.prettierrc.json`.
3. Fix the file below manually or describe what the tools would change.

```ts
var my_variable = "hello world"; // uses var, double quotes, bad name

function MyFunction(arg1, arg2) {
  // bad casing, no types
  console.log(arg1);
  // arg2 is never used
  if (true) {
    return my_variable;
  }
}
```
