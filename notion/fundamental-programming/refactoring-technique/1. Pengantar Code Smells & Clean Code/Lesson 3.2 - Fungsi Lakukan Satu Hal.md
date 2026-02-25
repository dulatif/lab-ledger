# 💡 CC03.2: Functions - Do One Thing, Do It Well

**Outline:**

- **The Smell (SEEI):** Identifying "The Grab-Bag Function" which has multiple responsibilities and hidden side effects.
- **The Refactor (PPP):** Applying the "Do One Thing" principle and Command-Query Separation.
- **Your Refactoring Mission (Production):** Time to decompose a function that's doing too much.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Smell (SEEI)**

**The Code Smell: The Grab-Bag Function (or The Swiss Army Knife)**
This is a deeper look at the "Long Method" smell. A function should do one thing, do it well, and do it only. This smell occurs when a function mixes different levels of abstraction or has hidden **"side effects."**

**The Negative Impact: Why are multi-purpose functions a problem?**

- **Unpredictable Side Effects:** Calling a function to get a value can unexpectedly change the state of the system (e.g., `getAndClearCache()`).
- **Impossible to Reuse:** You can't reuse validation logic without also triggering account-locking logic if they are fused together.
- **Hard to Test:** Testing a multi-purpose function requires complex mocks for every service it touches.
- **Violates SRP:** The function has more than one "reason to change."

**Example of the Smell: The `validateAndSaveUser` function**
This function's name already hints that it's doing at least two things.

```ts
interface User {
  name: string;
  email: string;
}

// A global or class-level variable.
let validationErrors: string[] = [];

function validateAndSaveUser(user: User, dbConnection: any): boolean {
  validationErrors = []; // A hidden side effect: it clears a global state.

  // 1. Validation Logic
  if (user.name.length < 3) {
    validationErrors.push("Name is too short.");
  }
  if (!user.email.includes("@")) {
    validationErrors.push("Invalid email format.");
  }

  // 2. Control Flow & Saving Logic
  if (validationErrors.length > 0) {
    return false;
  } else {
    dbConnection.save(user); // A second responsibility.
    return true;
  }
}
```

### **Part 2: Practice - The Refactor (PPP)**

**The Technique: Decompose and Separate Queries from Commands**

1. **Do One Thing:** If you can't describe what a function does without using the word "and," it's probably doing too much.
2. **Extract Method:** Extract logical blocks into their own well-named functions.
3. **Command-Query Separation (CQS):** A function should either be a **Command** (performs an action, changes state) or a **Query** (returns data, has no side effects). It should not be both.

**Step-by-Step Implementation: Refactoring `validateAndSaveUser`**
Let's separate the validation (a Query) from the saving (a Command).

```ts
// 1. Create a pure "Query" function for validation.
function validateUser(user: User): string[] {
  const errors: string[] = [];
  if (user.name.length < 3) {
    errors.push("Name is too short.");
  }
  if (!user.email.includes("@")) {
    errors.push("Invalid email format.");
  }
  return errors;
}

// 2. Create a pure "Command" function for saving.
function saveUser(user: User, dbConnection: any): void {
  dbConnection.save(user);
}

// 3. The calling code (the "client") now orchestrates the steps.
function handleUserRegistration(user: User, dbConnection: any) {
  const validationErrors = validateUser(user);
  if (validationErrors.length === 0) {
    saveUser(user, dbConnection);
  } else {
    console.error("Registration failed:", validationErrors);
  }
}
```

## 🧠 **Real-World Case Study: "Before vs. After Refactoring"**

- **Before (Mixed Responsibilities):**
  ```ts
  function validateAndSaveUser(user: User, db: any): boolean {
    // ... mixed logic with side effects ...
  }
  ```
- **After (Separated Responsibilities):**
  ```ts
  function validateUser(user: User): string[] {
    /* pure query */
  }
  function saveUser(user: User, db: any): void {
    /* pure command */
  }
  function handleUserRegistration(user: User, db: any) {
    /* orchestrator */
  }
  ```
- **The Improvement:**
  - **Testability:** `validateUser` is now trivial to test with no database mock needed.
  - **Reusability:** Use `validateUser` in front-end forms without triggering a database save.
  - **Clarity:** The code is simple, predictable, and free of hidden side effects.

## 🤔 **Reflective Questions**

1. What is a "side effect" in the context of a function?
2. How does the "Single Level of Abstraction Principle" relate to "Functions should do one thing"?
3. Can you think of a valid exception to the Command-Query Separation principle? (e.g., `stack.pop()`).

## 📚 **Further Reading**

- **The Bible:** "Clean Code," Chapter 3: "Functions."
- **Command-Query Separation:** Fowler's explanation of the CQS principle.
- **Functional Programming:** Pure functions and avoiding state changes.

## 📝 **Mini Task (Production)**

You are given a function that parses strings, filters inactive users, and saves active users.

**Your Mission:**
Refactor `processAndSaveUsers` into smaller functions: `parseUser`, `isActive`, and `saveUser`.

```ts
interface User {
  id: number;
  name: string;
  isActive: boolean;
}

function processAndSaveUsers(rawData: string[], dbConnection: any): number {
  let activeUserCount = 0;
  for (const line of rawData) {
    const parts = line.split(",");
    if (parts.length !== 3) continue;

    const user: User = {
      id: parseInt(parts[0]),
      name: parts[1],
      isActive: parts[2] === "true",
    };

    if (user.isActive) {
      dbConnection.save(user);
      activeUserCount++;
    }
  }
  return activeUserCount;
}
```
