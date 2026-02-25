# 💡 CC03.3: Komentar vs. Kode yang Jelas - Self-Documenting Code

**Outline:**

- **The Smell (SEEI):** Identifying "Deodorant Comments" - comments that apologize for or explain confusing code.
- **The Refactor (PPP):** Applying the principle of writing self-documenting code and knowing when comments are still valuable.
- **Your Refactoring Mission (Production):** Time to remove comments by improving the code they describe.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Smell (SEEI)**

**The Code Smell: Deodorant Comments**
A "deodorant comment" is one you write to explain a piece of code that is confusing, complex, or has a bad name. You are using the comment as an air freshener to cover up a "code smell." Other bad comments include:

- **Redundant Comments:** Stating the obvious (`// i is a counter`).
- **Misleading Comments:** Comments that are out of date and no longer reflect what the code actually does.
- **Commented-Out Code:** Dead code that clutters the file. (Use Git for this!)

**The Negative Impact: The Lie of "Good Comments"**
Most comments are a liability:

- **They Rot:** Code changes, but comments are rarely updated. An out-of-date comment is a trap.
- **They Hide Bad Code:** A comment often acts as an excuse to not refactor.
- **They Add Noise:** Redundant comments make files harder to parse visually.

**Example of the Smell: Explaining a complex expression**
The developer knew this code was hard to read, so they added a "deodorant" comment.

```ts
interface User {
  isLoggedIn: boolean;
  isSubscribed: boolean;
  isAdmin: boolean;
}

function canUserAccessContent(user: User): boolean {
  // Check if user is an admin or is logged in AND subscribed
  if (user.isAdmin || (user.isLoggedIn && user.isSubvinced)) {
    // Typo in variable name!
    return true;
  } else {
    return false;
  }
}
```

The comment describes the intent but hides the bug (a typo `isSubvinced`).

### **Part 2: Practice - The Refactor (PPP)**

**The Technique: Write Self-Documenting Code**
The best comment is no comment. Express the logic in the code itself.

1. **Extract Variable:** Extract complex expressions into variables with names that explain their purpose.
2. **Extract Method:** Extract blocks of code into new functions with descriptive names.
3. **Rename:** Use intention-revealing names for variables and functions.

**Step-by-Step Implementation: Refactoring the access check**
We can express the logic in a way that makes the comment unnecessary.

```ts
function canUserAccessContent(user: User): boolean {
  // 1. Extract parts into well-named variables.
  const isPaidUser = user.isLoggedIn && user.isSubscribed; // Typo is now obvious!
  const canAccess = user.isAdmin || isPaidUser;

  // 2. The code is now self-explanatory.
  return canAccess;
}
```

## 🧠 **Real-World Case Study: "Before vs. After Refactoring"**

- **Before (Comment explains unclear code):**
  ```ts
  // Check if eligible for free shipping
  if ((order.total > 50 && customer.isPreferred) || order.containsLargeItem) {
    //...
  }
  ```
- **After (Self-documenting code):**

  ```ts
  const hasFreeShippingByPrice = order.total > 50 && customer.isPreferred;
  const hasFreeShippingBySize = order.containsLargeItem;

  if (hasFreeShippingByPrice || hasFreeShippingBySize) {
    //...
  }
  ```

- **The Improvement:**
  - **Clarity:** The `if` statement reads like plain English.
  - **Maintainability:** The code is the single source of truth; no risk of out-of-sync comments.
  - **Debuggability:** Inspect `hasFreeShippingByPrice` directly during debugging.

## 🤔 **Reflective Questions**

1. What are examples of **good** comments? (e.g., explaining _why_ a workaround is needed).
2. How are documentation comments (JSDoc/TSDoc) different from "deodorant" comments?
3. How can focusing on comment-free code force you to become a better programmer?

## 📚 **Further Reading**

- **The Bible:** "Clean Code," Chapter 4: "Comments."
- **Refactoring:** Fowler's "Refactoring" catalog (e.g., "Introduce Explaining Variable").
- **Blog Post:** [Coding like you are trying to put yourself out of a job](https://www.google.com/search?q=https://blog.craftlab.hu/self-documenting-code-96ae330c6a83)

## 📝 **Mini Task (Production)**

You are given a function with several comments explaining confusing parts.

**Your Mission:**
Refactor the code to be self-documenting and **delete all the comments**.

```ts
function calculateFinalPrice(price: number, quantity: number, user: any) {
  // Calculate total price
  const total = price * quantity;

  // Apply discount for loyal users (users with > 5 orders)
  let discount = 0;
  if (user.orderCount > 5) {
    discount = total * 0.1; // 10% discount
  }

  // Calculate tax (8%)
  const tax = (total - discount) * 0.08;

  // Final price is total minus discount plus tax
  return total - discount + tax;
}
```
