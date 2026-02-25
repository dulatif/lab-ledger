# 💡 CS01.1: Apa Itu Code Smells?

**Outline:**

- **The Smell (SEEI):** Identifying and understanding the concept of a "Code Smell."
- **The "Refactor" (PPP):** Applying a new mindset: "Code Investigation."
- **Your Refactoring Mission (Production):** Time to start smelling some code.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Smell (SEEI)**

This is one of the most powerful capabilities of a developer: the ability to recognize problems before they become bugs. A **code smell** is a surface-level indicator in your code that might point to a deeper design problem. It's not a bug—the code works fine—but it's a characteristic that suggests your code could be simplified, more flexible, or easier to maintain. Think of it like a strange noise in your car's engine; the car still drives, but you know something might be wrong under the hood.

**The Negative Impact: Why Should We Care?**
Ignoring code smells leads to **technical debt**. Your codebase becomes difficult and slow to work with. Over time, this results in:

- **Reduced Productivity:** Simple changes take longer than they should.
- **Increased Bugs:** Modifying one part of the code breaks another unexpectedly.
- **Poor Onboarding:** New developers struggle to understand the messy codebase.
- **Low Morale:** Developers get frustrated working with fragile, complex code.

**Example of the Smell: A First Whiff**
Look at this TypeScript function. You don't need to know the specific name of the smell yet. Just read it and see how it _feels_.

```ts
// Processes a user order and sends a notification
function handleOrder(
  user: any,
  items: any[],
  creditCard: string,
  promoCode?: string,
) {
  // First, check user data
  if (!user.name || !user.address) {
    console.error("Invalid user data!");
    return;
  }

  // Then, calculate the total price
  let total = 0;
  for (const item of items) {
    total += item.price;
  }
  if (promoCode === "SAVE10") {
    total *= 0.9;
  }

  // Process payment
  console.log(`Charging ${creditCard} for amount: $${total}`);
  // ... imagine complex payment gateway logic here ...

  // Finally, send a confirmation email
  console.log(`Sending email to ${user.email}...`);
  // ... imagine email service logic here ...

  console.log("Order handled.");
}
```

Does this function feel a bit... much? It's doing a lot of different things. This "feeling" is your intuition picking up a code smell.

### **Part 2: Practice - The "Refactor" (PPP)**

**The Technique: Code Investigation Mindset**
For this first lesson, our "technique" isn't a code change. It's a change in mindset. The goal is to stop seeing code as just "working" or "not working" and start developing an intuition for code quality. We'll practice asking critical questions when we read code.

**Step-by-Step Implementation: How to Investigate**

1. **Read the Code's Purpose:** What is the function or class supposed to do? (e.g., `handleOrder`).
2. **Count the Responsibilities:** List all the _distinct jobs_ the code is performing.
   - It validates user data.
   - It calculates a total price.
   - It applies a discount.
   - It processes a payment.
   - It sends an email.
3. **Assess Readability:** How long did it take you to understand what's happening? Did you have to re-read it?
4. **Hypothesize the "Smell":** Give the problem a simple name. For the example above, you might call it "The Everything Function." The official name is "Long Method," which we'll cover next.

## 🧠 **Real-World Case Study: "Before vs. After Investigation"**

This isn't a code change, but a change in perception.

- **Before (The "It Works" Mindset):**
  > "The handleOrder function works. It takes order data and processes it. I ran it, and the logs look correct. It's done."
- **After (The "Code Investigator" Mindset):**
  > "The handleOrder function works, but it smells. It has at least four different responsibilities: validation, calculation, payment, and notification. If we need to change how emails are sent, we have to touch the same function that handles payment logic. This is risky. This function is a good candidate for refactoring because it's doing too much."
- **The Improvement:** By simply changing our mindset, we've identified a future risk without changing a single line of code. We have created a roadmap for improvement. This proactive identification is the first and most critical step in maintaining a healthy codebase.

## 🤔 **Reflective Questions**

1. Think about a piece of code you've written or worked with recently. Did it have any "smells"? What feeling did it give you (e.g., "confusing," "brittle," "too big")?
2. Why is the distinction between a "bug" and a "code smell" important for team communication?
3. How can identifying code smells early help in estimating the time required for future tasks?

## 📚 **Further Reading**

- **The Origin Story:** [Martin Fowler's original blog post on Code Smells](https://martinfowler.com/bliki/CodeSmell.html)
- **Code Quality Tool:** Get a feel for automated detection with [SonarLint for VS Code](https://www.sonarsource.com/products/sonarlint/), which can highlight smells directly in your editor.
- **SOLID Principles:** Code smells often violate fundamental design principles. Get a head start by reading about the [SOLID principles](https://www.google.com/search?q=https://www.digitalocean.com/community/conceptual_articles/s-o-l-i-d-the-first-5-principles-of-object-oriented-design).

## 📝 **Mini Task (Production)**

You are given a TypeScript class below. Your task is **not** to fix it. Instead, apply the "Code Investigation" technique.

**Your Mission:**

1. Read the code.
2. Write down a list of all the distinct responsibilities you think the `ReportGenerator` class has.
3. Give the "smell" a name of your own invention (e.g., "The God Class," "The Messy Drawer"). There's no wrong answer; the goal is to practice identification.

```ts
class ReportGenerator {
  private data: any[];

  constructor() {
    // 1. Fetches data from a database
    console.log("Connecting to database...");
    this.data = [
      { user: "Alice", value: 100 },
      { user: "Bob", value: 150 },
    ];
    console.log("Data fetched.");
  }

  public generateReport(type: "CSV" | "JSON") {
    if (type === "CSV") {
      // 2. Formats data as CSV
      console.log("Generating CSV report...");
      const csvHeader = "User,Value\n";
      const csvRows = this.data.map((d) => `${d.user},${d.value}`).join("\n");
      const fullCsv = csvHeader + csvRows;
      // 3. Writes the CSV to a file
      console.log("Saving report to report.csv");
      // fs.writeFileSync('report.csv', fullCsv);
      return fullCsv;
    } else if (type === "JSON") {
      // 4. Formats data as JSON
      console.log("Generating JSON report...");
      const jsonString = JSON.stringify(this.data, null, 2);
      // 5. Writes the JSON to a file
      console.log("Saving report to report.json");
      // fs.writeFileSync('report.json', jsonString);
      return jsonString;
    }
  }
}
```
