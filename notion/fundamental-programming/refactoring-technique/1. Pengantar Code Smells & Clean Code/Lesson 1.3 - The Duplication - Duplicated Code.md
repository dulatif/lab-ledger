# 💡 CS01.3: The Duplication - Duplicated Code

**Outline:**

- **The Smell (SEEI):** Identifying and understanding "Duplicated Code."
- **The Refactor (PPP):** Applying techniques to centralize logic and remove duplication.
- **Your Refactoring Mission (Production):** Time to DRY up some code.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Smell (SEEI)**

This is perhaps the most fundamental smell in software engineering. **Duplicated code** is the same, or very similar, code structure appearing in more than one place. It violates the "Don't Repeat Yourself" (DRY) principle. This duplication can be obvious (copy-paste) or subtle (two loops that iterate and filter differently but achieve a similar goal).

**The Negative Impact: The "Update in Two Places" Problem**
Duplication is a ticking time bomb for bugs and maintenance overhead.

- **Increased Bugs:** If you fix a bug in one place, you are very likely to forget to fix it in the other duplicated locations.
- **Increased Effort:** When a requirement changes, you have to find and update every single instance of that logic. This is slow and error-prone.
- **Reduced Clarity:** The codebase becomes bloated, and it's unclear which piece of code is the "single source of truth" for a piece of logic.

**Example of the Smell: Calculating Totals in Different Ways**
Imagine an e-commerce system where we calculate the total value of items for both a shopping cart and a final order. The logic is almost identical.

```ts
interface CartItem {
  price: number;
  quantity: number;
}

interface OrderLine {
  itemPrice: number;
  count: number;
}

function calculateCartTotal(items: CartItem[]): number {
  let total = 0;
  for (const item of items) {
    // Apply logic: maybe a discount or tax prep
    const itemTotal = item.price * item.quantity;
    total += itemTotal;
  }
  return total;
}

function calculateOrderTotal(lines: OrderLine[]): number {
  let total = 0;
  for (const line of lines) {
    // The exact same logic, just with different property names
    const lineTotal = line.itemPrice * line.count;
    total += lineTotal;
  }
  return total;
}
```

### **Part 2: Practice - The Refactor (PPP)**

**The Technique: Unify and Centralize (e.g., Extract Function, Introduce Parameter Object)**
The goal is to eliminate duplication by creating a single, reusable piece of code that all other parts can call.

1. **Identify the Duplication:** Find the blocks of code that look the same.
2. **Find the Differences:** Note the small variations between them (e.g., property names like `price` vs. `itemPrice`).
3. **Create a Unified Function:** Write a new function that contains the common logic.
4. **Parameterize the Differences:** Use function parameters to handle the variations. This might mean passing in the property names or, even better, making the data structures consistent.
5. **Replace Old Code:** Call the new, unified function from all the places that previously contained the duplicated code.

**Step-by-Step Implementation: Creating a Single `calculateTotal`**
The best first step is to make the data structures consistent. Then we can extract a single function.

```ts
// Step 1: Unify the interface
interface PricedItem {
  price: number;
  quantity: number;
}
// Now CartItem and OrderLine can be refactored to implement or be replaced by PricedItem.

// Step 2: Extract a single, reusable function
function calculateTotal(items: PricedItem[]): number {
  let total = 0;
  for (const item of items) {
    const itemTotal = item.price * item.quantity;
    total += itemTotal;
  }
  return total;
}

// Step 3: Use the new function everywhere
function calculateCartTotal(items: PricedItem[]): number {
  // Now it's just a simple call
  return calculateTotal(items);
}

function calculateOrderTotal(lines: PricedItem[]): number {
  // Duplication is gone!
  return calculateTotal(lines);
}
```

## 🧠 **Real-World Case Study: "Before vs. After Refactoring"**

Let's see the impact.

- **Before (Duplicated Code):**

  ```ts
  function calculateCartTotal(items: CartItem[]): number {
    let total = 0;
    for (const item of items) {
      /* ...logic... */
    }
    return total;
  }

  function calculateOrderTotal(lines: OrderLine[]): number {
    let total = 0;
    for (const line of lines) {
      /* ...same logic... */
    }
    return total;
  }
  ```

- **After (Centralized Logic):**

  ```ts
  // One function to rule them all
  function calculateTotal(items: PricedItem[]): number {
    let total = 0;
    for (const item of items) {
      /* ...logic... */
    }
    return total;
  }

  // Other functions become simple wrappers
  function calculateCartTotal(items: PricedItem[]): number {
    return calculateTotal(items);
  }
  // etc.
  ```

- **The Improvement:**
  - **Single Source of Truth:** If we need to add a global discount logic, we only have to change it in **one place**: `calculateTotal`.
  - **Reduced Code Size:** The codebase is smaller and easier to navigate.
  - **Increased Reliability:** There's zero chance of fixing a bug in the cart calculation but forgetting it in the order calculation.

## 🤔 **Reflective Questions**

1. Subtle duplication is harder to spot than direct copy-paste. What strategies could you use to find similar (but not identical) logic in a large codebase?
2. Duplicated code often happens when developers are working on different features under a tight deadline. How can team practices like code reviews help prevent this smell?
3. The acronym DRY stands for "Don't Repeat Yourself." What is the related principle, WET, sometimes humorously used to describe code with lots of duplication? (Hint: "Write Everything Twice").

## 📚 **Further Reading**

- **The Bible:** Fowler's _Refactoring_ book has many patterns for dealing with different kinds of duplication.
- **Code Quality Tool:** [PMD's Copy/Paste Detector (CPD)](https://www.google.com/search?q=https://pmd.github.io/latest/pmd_userdocs_cpd.html) is a classic tool that can be run on many languages, including TypeScript, to find duplicated code blocks.
- **Abstraction:** Removing duplication is an act of abstraction. Reading about the [Rule of Three](<https://en.wikipedia.org/wiki/Rule_of_three_(computer_programming)>) can provide a useful guideline on when to refactor duplication.

## 📝 **Mini Task (Production)**

You are given two functions that generate report summaries for active and inactive users. The logic for fetching and formatting the user's name and status is duplicated.

**Your Mission:**
Refactor the code to remove the duplication. Create a single, private helper function that handles the common logic and is called by both `getActiveUsersReport` and `getInactiveUsersReport`.

```ts
interface User {
  id: number;
  name: string;
  status: "active" | "inactive";
  lastLogin: Date;
}

const allUsers: User[] = [
  { id: 1, name: "Alice", status: "active", lastLogin: new Date("2023-10-01") },
  { id: 2, name: "Bob", status: "inactive", lastLogin: new Date("2023-05-15") },
  {
    id: 3,
    name: "Charlie",
    status: "active",
    lastLogin: new Date("2023-10-02"),
  },
];

function getActiveUsersReport(): string {
  const activeUsers = allUsers.filter((u) => u.status === "active");
  const reportLines: string[] = ["Active Users Report:"];
  for (const user of activeUsers) {
    // Duplicated formatting logic is here
    const status = `STATUS: ${user.status.toUpperCase()}`;
    const name = `NAME: ${user.name}`;
    reportLines.push(`- ${name} | ${status}`);
  }
  return reportLines.join("\n");
}

function getInactiveUsersReport(): string {
  const inactiveUsers = allUsers.filter((u) => u.status === "inactive");
  const reportLines: string[] = ["Inactive Users Report:"];
  for (const user of inactiveUsers) {
    // And also here!
    const status = `STATUS: ${user.status.toUpperCase()}`;
    const name = `NAME: ${user.name}`;
    reportLines.push(`- ${name} | ${status}`);
  }
  return reportLines.join("\n");
}
```
