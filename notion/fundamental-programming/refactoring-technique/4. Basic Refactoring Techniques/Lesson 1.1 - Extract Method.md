# 💡 BRT01.1: Extract Method

**Outline:**

- **The Smell (SEEI):** Identifying and understanding the "Long Method" code smell.
- **The Refactor (PPP):** Applying the "Extract Method" technique to fix the smell.
- **Your Refactoring Mission (Production):** Time to clean up some code.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Smell (SEEI)**

**The Code Smell: Long Method**
This is one of the most common smells. A "Long Method" (or function) has grown over time and is trying to do too many things. The key sign isn't just line count, but distinct responsibilities. If you're adding comments to explain different _sections_ of a function, you've found a Long Method.

**The Negative Impact**

- **Hard to Understand:** Too much context to follow logic from start to end.
- **Difficult to Reuse:** You can't take just one part of the logic elsewhere, forcing "Copy-Paste" duplication.
- **Fragile:** Changes in one part often have unintended side effects on another.
- **Hard to Test:** Testing one function that does 10 things requires massive setup and complex assertions.

**Example of the Smell**
A function that calculates an invoice, adding shipping, and printing details.

```ts
function processInvoice(order: Order) {
  let total = 0;

  // 1. Calculate the subtotal of all items
  console.log("--- Invoice Details ---");
  for (const item of order.items) {
    total += item.price * item.quantity;
    console.log(
      `${item.name} (x${item.quantity}): $${item.price * item.quantity}`,
    );
  }
  console.log(`Subtotal: $${total}`);

  // 2. Add shipping cost
  let shippingCost = order.shippingMethod === "express" ? 20 : 10;
  total += shippingCost;
  console.log(`Shipping: $${shippingCost}`);

  // 3. Print the final total and customer details
  console.log("---------------------");
  console.log(`TOTAL: $${total}`);
  console.log("---------------------");
  console.log(`Ship to: ${order.customer.name}`);
  console.log(`Address: ${order.customer.address}`);
}
```

### **Part 2: Practice - The Refactor (PPP)**

**The Technique: Extract Method**
Our primary tool for fighting Long Methods.

1. **Find a self-contained fragment** of code within the long method.
2. **Move it** into a new, separate method.
3. **Name it clearly** based on what it does (e.g., `calculateSubtotal`). Names often come from the comment explaining the block.
4. **Replace the original fragment** with a call to the new method.

**Step-by-Step Implementation**

1. **Extract Item Calculation:** Moves subtotal logic to its own function.
2. **Extract Shipping Calculation:** Moves branch logic to a dedicated helper.
3. **Extract Printing:** Groups various `console.log` statements.

The result is a `processInvoice` that reads like a high-level summary.

## 🧠 **Real-World Case Study: "Before vs. After Refactoring"**

- **Before (Long Method):**
  A single function doing calculation, routing (shipping), and I/O (printing).
- **After (Extract Method):**

  ```ts
  function processInvoice(order: Order) {
    const subtotal = calculateSubtotal(order);
    const shippingCost = calculateShipping(order);
    const total = subtotal + shippingCost;

    printInvoice(order, subtotal, shippingCost, total);
  }
  ```

- **The Improvement:** The main function is now a clear story. Details are abstracted into single-responsibility functions that are easier to test and potentially reuse.

## 🤔 **Reflective Questions**

1. How do unit tests make the Extract Method refactoring safer?
2. Which SOLID principle is most directly addressed by this technique?
3. How can your IDE help you automate the "Extract Method" process?

## 📚 **Further Reading**

- **The Bible:** Martin Fowler's "Refactoring" - Chapter 1.
- **Tools:** `SonarLint` for VS Code (detects Long Methods).
- **SOLID:** Single Responsibility Principle.

## 📝 **Mini Task (Production)**

Refactor this `registerUser` function by extracting validation, database, and email logic.

```ts
function registerUser(user: User) {
  // 1. Validate input
  if (!user.email.includes("@") || user.name.length < 2) {
    console.error("Invalid user data");
    return;
  }

  // 2. Save to database
  console.log(`Saving ${user.name}...`);
  // (complex DB logic)

  // 3. Send welcome email
  console.log(`Sending welcome email to ${user.email}...`);
  // (complex email logic)

  console.log("User registration complete!");
}
```
