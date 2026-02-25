# 💡 BRT01.3: Studi Kasus Extract Method

**Outline:**

- **The Smell (SEEI):** Understanding "Inappropriate Abstraction Level"—code that is either too monolithic or too fragmented.
- **The Refactor (PPP):** Applying Extract and Inline Method iteratively to find the right balance.
- **Your Refactoring Mission (Production):** Time to find the sweet spot in a complex function.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Smell (SEEI)**

**The Code Smell: Inappropriate Abstraction Level**
This is a subtle smell. It's not about being definitively "too long" or "too short," but about the design feeling wrong.

- **Too Monolithic:** Distinct concepts are buried in one large function. Lack of abstraction.
- **Too Fragmented:** Logic is spread across too many tiny functions (Abstraction Obsession). You jump through 10 files for one workflow.

**The Negative Impact**

- **Monolithic Code:** Hard to read, reuse, and test (Long Method syndrome).
- **Fragmented Code:** Navigation hell. Hard to see the high-level picture because you're constantly diving into implementation details.

**Example of the Smell**
A shopping cart processor that does validation, calculation, and formatting in one go. Good for a "refactoring dance."

```ts
function getCartTotal(items: CartItem[], user: User): string {
  // 1. Validation
  if (!items || items.length === 0) throw new Error("Cart is empty");

  // 2. Base Calculation
  let total = 0;
  for (const item of items) {
    if (item.price < 0 || item.quantity <= 0) throw new Error("Invalid item");
    total += item.price * item.quantity;
  }

  // 3. Discount & Formatting
  if (user.isPremium) total *= 0.9;
  return `Total: $${total.toFixed(2)}`;
}
```

### **Part 2: Practice - The Refactor (PPP)**

**The Technique: The Extract-and-Inline Dance**
Refactoring isn't a one-way street; it's an experiment to find clarity.

1. **First Pass (Aggressive Extract):** Extract every distinct concept to discover hidden domain logic.
2. **Second Pass (Evaluate & Inline):** If the result is too fragmented or functions are mere noise, inline them back until you find the perfect rhythm.

**Step-by-Step Implementation**

1. **Over-Extract:** Separate `validateIsNotEmpty`, `validateItems`, `calculateBase`, `applyDiscount`, `format`.
2. **Finding Balance:** Realizing `validateIsNotEmpty` and `validateItems` are always called together. Inline them into a single `validateCart` function.

## 🧠 **Real-World Case Study: "Before vs. After Refactoring"**

- **Before (Monolithic):**
  One block of code handling all logic, making it hard to read and branch.
- **After (Balanced Abstraction):**
  ```ts
  function getCartTotal(items: CartItem[], user: User): string {
    validateCart(items);
    const baseTotal = calculateBaseTotal(items);
    const discountedTotal = applyDiscount(baseTotal, user);
    return formatTotal(discountedTotal);
  }
  ```
- **The Improvement:** The top-level function is now a high-level summary. Each step is delegated to a function at the right level of abstraction. The code is readable, testable, and reusable.

## 🤔 **Reflective Questions**

1. What rules of thumb do you use to decide if a function is the "right" size?
2. How does team familiarity affect the ideal abstraction level?
3. Can a function be "too short"? What does that look like?

## 📚 **Further Reading**

- **The Bible:** Martin Fowler's "Refactoring" (Extract & Inline together).
- **Sandi Metz:** "The Art of Abstraction" (The dangers of abstracting too early).
- **Tools:** `Code Climate` for complexity analysis.

## 📝 **Mini Task (Production)**

Refactor this `generateActivityReport` function. Don't over-extract; find the right balance using **Extract** and **Inline**.

```ts
function generateActivityReport(userId: string, fromDate: Date): string {
  // 1. Fetch and filter
  const activities = fetchActivities(userId).filter(
    (a) => a.timestamp > fromDate,
  );
  if (activities.length === 0) return "No activity found.";

  // 2. Group by type
  const grouped = activities.reduce(
    (acc, a) => {
      acc[a.type]++;
      return acc;
    },
    { LOGIN: 0, LOGOUT: 0, PURCHASE: 0 },
  );

  // 3. Format
  return `Report for ${userId}:\nLogins: ${grouped.LOGIN}\nLogouts: ${grouped.LOGOUT}\n...`;
}
```

Explain why you chose your final level of abstraction.
