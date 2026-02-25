# 💡 ACS01.3: Gejala dan Konsekuensi

**Outline:**

- **The Smell (SEEI):** Understanding Low Cohesion and High Coupling as the direct results of Large Classes.
- **The "Refactor" (PPP):** Applying "Cohesion Analysis" by mapping method and property interactions.
- **Your Refactoring Mission (Production):** Time to analyze a class and diagnose its cohesion problems.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Smell (SEEI)**

**The Code Smell: Low Cohesion & High Coupling**
These are fundamental design problems caused by Large Classes and God Objects:

- **Low Cohesion:** A class has low cohesion if its responsibilities are unrelated. An `Order` class with methods for `calculateTotal()` and `connectToSmtpServer()` has very low cohesion.
- **High Coupling:** Highly dependent modules where a change in one requires a change in the other. If a `ReportGenerator` directly uses the internal data of a `User`, they are tightly coupled.

**The Negative Impact: The Ripple Effect of Death**
Low cohesion and high coupling create brittle systems:

- **The Ripple Effect:** A small change in a highly-coupled system causes a cascade of bugs.
- **Reduced Reusability:** You can't reuse a module because it's too dependent on another.
- **Difficult to Understand:** You can't understand a class in isolation if it has unrelated responsibilities.
- **"Shotgun Surgery":** A symptom where one logical change requires many small edits across many different classes.

**Example of the Smell: The `User` class**
This class suffers from low cohesion by mixing data with presentation and database logic.

```ts
class User {
  // Core User Data (High Cohesion)
  public id: string;
  public name: string;
  public email: string;

  // Unrelated Presentation Logic (Low Cohesion)
  private detailView: any;
  public renderToDom() {
    /* ... */
  }

  // Unrelated Persistence Logic (Low Cohesion)
  private dbConnection: any;
  public save() {
    /* ... */
  }
}
```

The `User` class has three reasons to change: data definition, rendering framework, or database schema.

### **Part 2: Practice - The "Refactor" (PPP)**

**The Technique: Cohesion Analysis**
Cohesion Analysis helps identify multiple responsibilities by mapping which methods use which instance variables.

1. **Create a table:** Rows are methods, columns are instance variables.
2. **Mark interaction:** For each method, mark which variables it reads/writes.
3. **Look for clusters:** Distinct groups of marks identify separate responsibilities.

**Step-by-Step Implementation: Analyzing the `User` class**

| Method/Property | `id` | `name` | `email` | `detailView` | `dbConnection` |
| --------------- | ---- | ------ | ------- | ------------ | -------------- |
| `renderToDom()` | X    | X      |         | X            |                |
| `save()`        | X    | X      | X       |              | X              |

- **Analysis:**
  - `renderToDom()` uses core data and `detailView`. (Presentation group)
  - `save()` uses core data and `dbConnection`. (Persistence group)

This confirms that Presentation and Persistence should be extracted into classes like `UserView` and `UserRepository`.

## 🧠 **Real-World Case Study: "Before vs. After Analysis"**

- **Before (Low Cohesion, High Coupling):**
  > A `Product` class contains pricing, inventory, and marketing logic. Changing the inventory system might break marketing copy generation.
- **After (High Cohesion, Low Coupling):**
  > The system is refactored into: `Product` (data), `InventoryService` (logic), and `MarketingCopyGenerator` (logic).
- **The Improvement:**
  - **Robustness:** Changes are localized; the ripple effect is contained.
  - **Clarity:** Each class has a singular purpose.
  - **Parallel Development:** Teams work on separate services without merge conflicts in a single God class.

## 🤔 **Reflective Questions**

1. Can you think of a non-software example of something with high vs. low cohesion?
2. What's the intuitive difference between data coupling and control flow coupling?
3. How do access modifiers (`private`, `public`) help manage coupling?

## 📚 **Further Reading**

- **The Concepts:** Search for "Software Design Cohesion and Coupling."
- **GRASP Principles:** General Responsibility Assignment Software Patterns.
- **Structured Design:** These concepts originated in the 1970s.

## 📝 **Mini Task (Production)**

Perform a "Cohesion Analysis" on the `Document` class below.

**Your Mission:**

1. Create a table of methods vs. variables.
2. Identify at least two distinct responsibilities.
3. Name the responsibilities.

```ts
class Document {
  private content: string;
  private fileName: string;
  private fileFormat: "txt" | "md";
  private isCached: boolean = false;
  private cacheKey: string;

  constructor(name: string, content: string) {
    this.fileName = name;
    this.content = content;
    this.fileFormat = name.endsWith(".md") ? "md" : "txt";
    this.cacheKey = `doc:${this.fileName}`;
  }

  public wordCount(): number {
    return this.content.split(" ").length;
  }

  public saveToCache(): void {
    console.log(`Saving ${this.cacheKey} to cache.`);
    this.isCached = true;
  }

  public loadFromCache(): void {
    console.log(`Checking cache for ${this.cacheKey}.`);
  }

  public getFileName(): string {
    return this.fileName;
  }
}
```
