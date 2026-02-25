# 💡 ACS03.4: Duplicated Code Revisited

**Outline:**

- **The Smell (SEEI):** Identifying subtle, non-literal duplication where algorithms or structures are similar but not identical.
- **The Refactor (PPP):** Applying the "Form Template Method" design pattern to unify similar algorithms.
- **Your Refactoring Mission (Production):** Time to refactor similar processes using a template method.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Smell (SEEI)**

**The Code Smell: Subtle or "Hidden" Duplication**
Duplication is often more subtle than copy-pasting. Two methods might follow similar steps with slightly different details. This is duplicated _knowledge_ or _process_, violating DRY at a higher level of abstraction.

**The Negative Impact: The Difficulty of Process Changes**

- **Brittle Process:** Changing the overall algorithm (e.g., adding logging) requires updates in every implementation of the duplicated process.
- **Inconsistent Implementation:** Implementations diverge over time; one might end up with better error handling than another.
- **Hidden Relationships:** The shared high-level process is only in developers' heads, not explicitly enforced by code.

**Example of the Smell: Similar Report Generation**
PDF and CSV reports follow the same skeleton: initialize, format header, body, and footer.

```ts
class PdfReportGenerator {
  public generate(): string {
    let report = "PDF_INIT\n";
    report += "PDF Header\n";
    report += "PDF Body Content\n";
    report += "PDF Footer\n";
    return report;
  }
}

class CsvReportGenerator {
  public generate(): string {
    let report = "";
    report += "col1,col2,col3\n";
    report += "val1,val2,val3\n";
    report += "CSV Footer Note\n";
    return report;
  }
}
```

### **Part 2: Practice - The Refactor (PPP)**

**The Technique: Form Template Method**
A classic pattern for removing process duplication.

1. **Create a Base Class:** Contains the shared algorithm.
2. **Create the Template Method:** A public method defining the skeleton, calling other methods for steps.
3. **Define Abstract Methods:** For varying steps, define `abstract` methods in the base class.
4. **Create Concrete Subclasses:** Subclasses implement the abstract details.

**Step-by-Step Implementation: Creating a Report Template**

```ts
abstract class ReportGenerator {
  // The "Template Method"
  public generate(): string {
    let report = this.initialize();
    report += this.formatHeader();
    report += this.formatBody();
    report += this.formatFooter();
    return report;
  }

  protected abstract initialize(): string;
  protected abstract formatHeader(): string;
  protected abstract formatBody(): string;
  protected abstract formatFooter(): string;
}

class PdfReportGenerator extends ReportGenerator {
  protected initialize(): string {
    return "PDF_INIT\n";
  }
  protected formatHeader(): string {
    return "PDF Header\n";
  }
  protected formatBody(): string {
    return "PDF Body Content\n";
  }
  protected formatFooter(): string {
    return "PDF Footer\n";
  }
}
```

## 🧠 **Real-World Case Study: "Before vs. After Refactoring"**

- **Before (Duplicated Process):**
  > Two data importers (Users, Products) share steps: open file, parse, validate, save, close.
- **After (Template Method):**
  > `BaseImporter` has `run()` template method. `UserImporter` and `ProductImporter` subclasses implement specific `parseData()`, `validateRow()`, and `saveRecord()` methods.
- **The Improvement:**
  - **DRY Principle:** High-level algorithm defined in one place.
  - **Clear Structure:** Explicitly separates invariant (skeleton) from variant parts.
  - **Extensibility:** Adding new importers (e.g., `JsonReportGenerator`) is easy and follows the established sequence.

## 🤔 **Reflective Questions**

1. Template Method vs. Strategy pattern: When to choose which?
2. What if a "primitive" method in the base class has a default implementation instead of being abstract?
3. How can duplication detection tools help find subtle, structural duplication?

## 📚 **Further Reading**

- **Design Patterns:** "Template Method" - one of the 23 GoF patterns.
- **Refactoring.guru:** [Template Method visual explanations](https://refactoring.guru/design-patterns/template-method).
- **The Bible:** Fowler's "Refactoring" - "Form Template Method" technique.

## 📝 **Mini Task (Production)**

Refactor the `PhysicalOrderProcessor` and `DigitalOrderProcessor` below using "Form Template Method".

**Your Mission:**

1. Create an abstract `OrderProcessor` base class.
2. Define a `processOrder()` template method.
3. Identify and abstract the varying "delivery" step.
4. Implement subclasses.

```ts
class PhysicalOrderProcessor {
  public processOrder(order: any) {
    console.log("Validating payment...");
    console.log("Updating inventory...");
    console.log("Creating a shipping label..."); // Varies
    console.log("Notifying customer...");
  }
}

class DigitalOrderProcessor {
  public processOrder(order: any) {
    console.log("Validating payment...");
    console.log("Updating inventory...");
    console.log("Generating a download link..."); // Varies
    console.log("Notifying customer...");
  }
}
```
