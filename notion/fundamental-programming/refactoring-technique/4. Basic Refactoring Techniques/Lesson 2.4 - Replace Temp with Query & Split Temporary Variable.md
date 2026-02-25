# 💡 BRT02.4: Replace Temp with Query & Split Variable

**Outline:**

- **The Smell (SEEI):** Identifying "Reused Temps" and long, complex calculations.
- **The Refactor (PPP):** Applying "Split Temporary Variable" and "Replace Temp with Query."
- **Your Refactoring Mission (Production):** Time to clean up some messy variables.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - Split Temporary Variable (SEEI)**

**The Code Smell: Reused Temporary Variable**
This occurs when a single variable (often named `temp`, `acc`, or `result`) is used for multiple, distinct purposes. It gets a value, is used, then reset for a completely different purpose.

**The Negative Impact**

- **Confusing:** The meaning of the variable changes as you read, forcing mental state tracking.
- **Prevents `const`:** You're forced to use `let`, which signals potential mutability and makes code harder to reason about.
- **Hard to Refactor:** Extracting logic is difficult because the variable has different meanings before and after a block.

**Example of the Smell**
A `temp` variable used first for perimeter, then for area.

```ts
let temp = 2 * (height + width);
console.log(`Perimeter: ${temp}`);

temp = height * width; // Now it's area! Confusing.
console.log(`Area: ${temp}`);
```

### **Part 2: Presentation - Replace Temp with Query (SEEI)**

**The Code Smell: Long-Lived or Complex Temp**
A temporary variable storing a calculation becomes a problem when:

1. The calculation logic clutters the main function.
2. The variable is declared far from its usage.
3. It creates dependencies that prevent method extraction.

**The Technique: Replace Temp with Query**
Move the calculation into its own private method or function (a "Query").

**Example of the Smell**

```ts
public calculateTotal(): number {
  const basePrice = this.quantity * this.itemPrice; // Clutters main logic
  const discountFactor = basePrice > 1000 ? 0.95 : 1;
  return basePrice * discountFactor;
}
```

### **Part 3: Practice - The Refactor (PPP)**

**The Refactor: Split & Query**

1. **Split Variable:** Create a new, distinct `const` for each responsibility (e.g., `perimeter`, `area`).
2. **Replace Template with Query:** Create a `get basePrice()` method. Replace variable usage with method calls.

**After Refactoring**

```ts
private get basePrice() { return this.quantity * this.itemPrice; }

public calculateTotal() {
  const discountFactor = this.basePrice > 1000 ? 0.95 : 1;
  return this.basePrice * discountFactor;
}
```

**The Improvement:** The main logic focuses on the "What" (applying a discount), not the "How" (calculating base price).

## 🧠 **Real-World Case Study: "Before vs. After Refactoring"**

- **Before (Messy Variables):**
  A developer uses `result` to store temporary numbers, then a string, then a formatted report. Any code extraction fails because `result` is shared.
- **After (Split and Query):**
  Calculations are moved to queries. The final report string is built using its own dedicated variable.
- **The Improvement:** The code is safer, easier to read, and modular. `const` is used everywhere, providing a guarantee that data doesn't change unexpectedly.

## 🤔 **Reflective Questions**

1. When is "Explain Variable" better than "Replace Temp with Query"?
2. How does using `const` by default help identify the "Reused Variable" smell?
3. What are the performance implications of "Replace Temp with Query"?

## 📚 **Further Reading**

- **The Bible:** Martin Fowler's "Refactoring".
- **Refactoring.Guru:** [Split Temporary Variable](https://refactoring.guru/split-temporary-variable) & [Replace Temp with Query](https://refactoring.guru/replace-temp-with-query).

## 📝 **Mini Task (Production)**

Refactor `generateStatement` using **Split Variable** and **Replace Temp with Query**.

```ts
function generateStatement(rentals: Rental[]) {
  let totalAmount = 0;
  let result = `Rental Record:\n`; // SMELL: String builder

  for (const rental of rentals) {
    const thisAmount = calculateAmount(rental); // REPLACE WITH QUERY
    result += `\t${rental.movie.title}\t${thisAmount}\n`;
    totalAmount += thisAmount;
  }

  const points = calculatePoints(rentals); // SPLIT INTO OWN VARIABLE
  result += `Amount owed is ${totalAmount}\nYou earned ${points} points`;
  console.log(result);
}
```

**Hint:** Extract the `switch` (movie pricing) and `points` logic into their own functions.
