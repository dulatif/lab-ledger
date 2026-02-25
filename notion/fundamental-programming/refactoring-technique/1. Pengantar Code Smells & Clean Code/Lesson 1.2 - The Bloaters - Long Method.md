# 💡 CS01.2: The Bloaters - Long Method

**Outline:**

- **The Smell (SEEI):** Identifying and understanding the "Long Method" code smell.
- **The Refactor (PPP):** Applying the "Extract Method" technique to fix it.
- **Your Refactoring Mission (Production):** Time to clean up a long function.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Smell (SEEI)**

This is one of the most common and easiest smells to spot. A **"Long Method"** (or "Long Function") is a procedure that has grown too large. There's no strict rule for "how long is too long," but a good heuristic is: if you need to scroll to see the whole function, it's probably too long. More importantly, if the function is doing more than one conceptual thing, it's a "Long Method."

**The Negative Impact: Why is it a Problem?**
Long methods are maintenance nightmares.

- **Hard to Understand:** You have to hold too much context in your head to figure out what the code is doing.
- **Difficult to Reuse:** You can't reuse one piece of the logic (e.g., just the calculation part) without running the whole method.
- **Prone to Bugs:** Changing one small part has a high risk of breaking something else in the same method. Duplicated logic often hides inside long methods.
- **Hard to Test:** Writing a unit test for a function that does 10 things requires 10 different test setups and assertions in one test case, which is a bad practice.

**Example of the Smell: The "All-in-One" Invoice Printer**
This function tries to do everything: calculate totals, find outstanding balances, and format a printable invoice.

```ts
interface Customer {
  name: string;
  invoices: { amount: number; isPaid: boolean }[];
}

function printInvoice(customer: Customer, dueDate: Date) {
  let outstanding = 0;
  let total = 0;

  // 1. Calculate outstanding and total
  console.log("***********************");
  console.log("***** Customer Invoice *****");
  console.log("***********************");
  for (const invoice of customer.invoices) {
    if (!invoice.isPaid) {
      outstanding += invoice.amount;
    }
    total += invoice.amount;
  }

  // 2. Print customer details
  console.log(`name: ${customer.name}`);
  console.log(`total: $${total}`);
  console.log(`outstanding: $${outstanding}`);

  // 3. Print due date
  const today = new Date();
  dueDate.setDate(today.getDate() + 30);
  console.log(`due: ${dueDate.toLocaleDateString()}`);
}
```

### **Part 2: Practice - The Refactor (PPP)**

**The Technique: Extract Method**
"Extract Method" is the primary refactoring technique for curing a Long Method. The process is simple:

1. **Find a logical chunk** of code within the long method.
2. **Move that code** into a new, private method.
3. **Name the new method** after its _intent_—what it _does_, not _how_ it does it.
4. **Replace the old code** with a call to the new method.

**Step-by-Step Implementation: Breaking Down the Invoice Printer**
We can identify three distinct responsibilities in our `printInvoice` function: printing the header, calculating amounts, and printing the details. Let's extract them.

```ts
// After refactoring
function printInvoice(customer: Customer, dueDate: Date) {
  printHeader();
  const { outstanding, total } = calculateOutstandingAndTotal(customer);
  printDetails(customer.name, total, outstanding, dueDate);
}

// New, extracted function for the header
function printHeader(): void {
  console.log("***********************");
  console.log("***** Customer Invoice *****");
  console.log("***********************");
}

// New, extracted function for calculation
function calculateOutstandingAndTotal(customer: Customer): {
  outstanding: number;
  total: number;
} {
  let outstanding = 0;
  let total = 0;
  for (const invoice of customer.invoices) {
    if (!invoice.isPaid) {
      outstanding += invoice.amount;
    }
    total += invoice.amount;
  }
  return { outstanding, total };
}

// New, extracted function for printing details
function printDetails(
  name: string,
  total: number,
  outstanding: number,
  dueDate: Date,
): void {
  const today = new Date();
  dueDate.setDate(today.getDate() + 30);

  console.log(`name: ${name}`);
  console.log(`total: $${total}`);
  console.log(`outstanding: $${outstanding}`);
  console.log(`due: ${dueDate.toLocaleDateString()}`);
}
```

## 🧠 **Real-World Case Study: "Before vs. After Refactoring"**

Let's look at the direct comparison.

- **Before (Long Method):**
  ```ts
  function printInvoice(customer: Customer, dueDate: Date) {
    let outstanding = 0;
    let total = 0;
    console.log("***********************");
    // ... 15 more lines of mixed logic ...
  }
  ```
- **After (Extract Method):**
  ```ts
  function printInvoice(customer: Customer, dueDate: Date) {
    printHeader();
    const { outstanding, total } = calculateOutstandingAndTotal(customer);
    printDetails(customer.name, total, outstanding, dueDate);
  }
  // ... with 3 smaller, well-named helper functions
  ```
- **The Improvement:**
  - **Readability:** The main `printInvoice` function now reads like a high-level summary of its steps. It's instantly understandable.
  - **Reusability:** We can now reuse `calculateOutstandingAndTotal` elsewhere in the application without needing to print anything.
  - **Testability:** We can write a specific unit test for `calculateOutstandingAndTotal` to verify its logic, separate from any console output.

## 🤔 **Reflective Questions**

1. How does having a good suite of unit tests make the process of refactoring with "Extract Method" safer and more efficient?
2. Which SOLID principle is most directly addressed by the "Extract Method" technique? (Hint: Single Responsibility Principle).
3. How can modern IDEs (like VS Code or WebStorm) automate the "Extract Method" refactoring for you? (Hint: Look for a "Refactor..." context menu).

## 📚 **Further Reading**

- **The Bible:** [Refactoring: Improving the Design of Existing Code by Martin Fowler](https://martinfowler.com/books/refactoring.html) - This is the definitive guide.
- **Code Quality Tool:** [ESLint with the SonarJS plugin](https://github.com/SonarSource/eslint-plugin-sonarjs) has a specific rule (`cognitive-complexity`) that helps detect methods that are too complex (and often too long).
- **SOLID Principles:** Re-read the [Single Responsibility Principle](https://en.wikipedia.org/wiki/Single-responsibility_principle). "Extract Method" is this principle applied at the function level.

## 📝 **Mini Task (Production)**

You are given a single, long TypeScript function that processes a user registration form. It handles validation, saving the user to a database, and sending a welcome email.

**Your Mission:**
Refactor this function by extracting the validation, database saving, and email sending logic into their own separate, well-named methods. The goal is to make the main `registerUser` function a short, high-level summary of the registration process.

```ts
interface User {
  name: string;
  email: string;
  dateOfBirth: Date;
}

function registerUser(user: User) {
  // 1. Validate User
  if (!user.name || user.name.length < 2) {
    console.error("Invalid name.");
    return;
  }
  if (!user.email || !user.email.includes("@")) {
    console.error("Invalid email.");
    return;
  }
  const age = new Date().getFullYear() - user.dateOfBirth.getFullYear();
  if (age < 18) {
    console.error("User must be 18 or older.");
    return;
  }
  console.log("User validation successful.");

  // 2. Save user to database
  console.log(`Saving ${user.name} to the database...`);
  // ... imagine database call logic here ...
  console.log("User saved.");

  // 3. Send welcome email
  console.log(`Sending welcome email to ${user.email}...`);
  // ... imagine email service logic here ...
  console.log("Email sent.");

  console.log("User registration complete!");
}
```
