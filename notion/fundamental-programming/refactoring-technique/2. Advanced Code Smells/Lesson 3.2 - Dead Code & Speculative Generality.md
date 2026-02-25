# 💡 ACS03.2: Dead Code & Speculative Generality

**Outline:**

- **The Smell (SEEI):** Identifying code that is never used (Dead Code) and features built "just in case" (Speculative Generality).
- **The Refactor (PPP):** Applying "Safe Deletion" using IDE tools and version control.
- **Your Refactoring Mission (Production):** Time to clean up some unused code.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Smell (SEEI)**

**The Code Smell: Dead Code & Speculative Generality**
These smells are about code that provides no value:

- **Dead Code:** A variable, parameter, method, or class that is never used. This often happens after refactoring where old code is left "just in case."
- **Speculative Generality:** Code created for a future requirement that never happened. Using an abstract class or interface when a simple one would suffice (YAGNI violation).

**The Negative Impact: Clutter and Confusion**

- **Increased Cognitive Load:** Mental energy is wasted on figuring out who uses code that's actually dead.
- **Maintenance Cost:** Dead code still needs to be read and maintained.
- **Fear of Deletion:** Leads to endless bloat. Developers fear deleting code thinking it might be used somewhere hidden.

**Example of the Smell: The Unused Exporter**
A feature to export XML was planned but never used. The supporting code remains.

```ts
class Report {
  constructor(public data: any[]) {}

  public generateCsv(): string {
    /* ... */
  }

  // DEAD CODE: Never called.
  public generateXml(): string {
    return "<report></report>";
  }

  // SPECULATIVE GENERALITY: includeHeader is always called with true.
  public formatData(includeHeader: boolean): string[] {
    return [];
  }
}
```

### **Part 2: Practice - The Refactor (PPP)**

**The Technique: Safe Deletion**
The refactoring is simply to delete. Use tools to do it safely.

1. **Find Usages:** Use your IDE to find references. If results are zero, it's a candidate for deletion.
2. **Delete and Compile:** The TypeScript compiler will catch broken links.
3. **Run Tests:** A safety net to ensure logic remains intact.
4. **Trust Version Control:** Use Git, not commented-out code. You can always revert.

**Step-by-Step Implementation: Cleaning the `Report` class**

1. **`generateXml()`:** 0 usages. **Delete the method.**
2. **`includeHeader` parameter:** Always called with `true`.
   - Use "Inline Variable" to replace parameter with `true` inside the method.
   - Use "Change Signature" to remove the parameter from the definition and call sites.
3. Run tests and commit with a clear message.

## 🧠 **Real-World Case Study: "Before vs. After Refactoring"**

- **Before (Code with Dead Method):**
  ```ts
  class UserProfile {
    public saveProfile() {
      /* ... */
    }
    public legacyValidateProfile(): boolean {
      return false;
    } // Never deleted
  }
  ```
- **After (Cleaned Code):**
  ```ts
  class UserProfile {
    public saveProfile() {
      /* ... */
    }
  }
  ```
- **The Improvement:** The class is smaller, simpler, and more honest. Cognitive load is reduced; new developers won't waste time on legacy methods.

## 🤔 **Reflective Questions**

1. What is the YAGNI principle?
2. Why is commenting out code worse than deleting it and using Git?
3. How can code coverage tools help identify dead code?

## 📚 **Further Reading**

- **The Bible:** Fowler's "Refactoring" - "Remove Parameter" and "Inline Function."
- **Refactoring.guru:** [Dead Code](https://refactoring.guru/smells/dead-code) and [Speculative Generality](https://refactoring.guru/smells/speculative-generality) smells.
- **YAGNI:** Martin Fowler’s article explains the "You Ain't Gonna Need It" principle.

## 📝 **Mini Task (Production)**

Identify and safely remove all Dead Code and Speculative Generality from the `DataProcessor` class below.

**Your Mission:**

1. List the elements to be removed.
2. Explain the safety steps.

```ts
const PI = 3.14; // Unused

class DataProcessor {
  private apiVersion: string = "v2"; // Never read

  public fetchData(source: "api" | "db"): any[] {
    return [];
  }

  public oldFormatData(data: any[]): string {
    return data.join(",");
  }

  public processData(data: any[], options?: { strict: boolean }): any[] {
    console.log("Processing data..."); // options are never used
    return data.map((item) => ({ ...item, processed: true }));
  }
}
```
