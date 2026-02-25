# 💡 SOLID02.4: I - The Interface Segregation Principle

**Outline:**

- **The Smell (SEEI):** Identifying the "Fat Interface" that forces classes to implement unnecessary methods.
- **The Refactor (PPP):** Applying the Interface Segregation Principle (ISP) to create smaller, more focused interfaces.
- **Your Refactoring Mission (Production):** Time to slim down a fat interface.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Smell (SEEI)**

**The Code Smell: The Fat Interface (or Polluted Interface)**
This smell occurs when an interface becomes too "fat"—it includes too many method declarations that are not all relevant to all implementers. This forces classes to implement methods that they don't need, often with an empty body or by throwing an exception.

**The Negative Impact: Why is a fat interface a problem?**
It creates unnecessary coupling and confusion:

- **Forces Unneeded Implementation:** Classes are burdened with methods that have no meaning for them.
- **Leads to LSP Violations:** Implementing a method by throwing an exception is a classic sign of a fat interface and violates the Liskov Substitution Principle.
- **Confusion for Clients:** When a developer sees a long list of available methods, it's not clear which ones are safe to call.
- **Poor API Design:** It indicates that the interface is mixing multiple responsibilities.

**Example of the Smell: The `IMachine` Interface**
Imagine a general-purpose interface for office machines. It's too broad and forces a simple printer to have a `staple` method.

```ts
// This is a FAT interface.
interface IMachine {
  print(document: any): void;
  scan(document: any): void;
  staple(document: any): void;
}

// The old, cheap printer is forced to implement methods it doesn't support.
class OldCheapPrinter implements IMachine {
  public print(doc: any) {
    console.log("Printing...");
  }

  public scan(doc: any) {
    // This is the problem! It can't scan.
    throw new Error("This machine does not support scanning.");
  }

  public staple(doc: any) {
    throw new Error("This machine does not support stapling.");
  }
}
```

### **Part 2: Practice - The Refactor (PPP)**

**The Technique: The Interface Segregation Principle (ISP)**
The ISP states: **"Clients should not be forced to depend upon interfaces that they do not use."**

The solution is to break down fat interfaces into smaller, more cohesive, role-based interfaces. A class can then implement multiple smaller interfaces instead of one large one.

**Step-by-Step Implementation: Segregating the `IMachine` Interface**
We can create a separate interface for each role: printing, scanning, and stapling.

```ts
// Step 1: Break the fat interface into smaller interfaces.
interface IPrinter {
  print(document: any): void;
}

interface IScanner {
  scan(document: any): void;
}

interface IStapler {
  staple(document: any): void;
}

// Step 2: Implement only the interfaces that are needed.
class OldCheapPrinter implements IPrinter {
  public print(doc: any) {
    console.log("Printing...");
  }
}

class MultiFunctionPrinter implements IPrinter, IScanner, IStapler {
  public print(doc: any) {
    console.log("Printing...");
  }
  public scan(doc: any) {
    console.log("Scanning...");
  }
  public staple(doc: any) {
    console.log("Stapling...");
  }
}
```

## 🧠 **Real-World Case Study: "Before vs. After Refactoring"**

- **Before (Fat Interface):**
  ```ts
  function performScan(machine: IMachine, doc: any) {
    // This client code is unsafe. It might get a machine that can't scan.
    machine.scan(doc);
  }
  ```
- **After (Segregated Interfaces):**

  ```ts
  // This client code is now 100% safe.
  function performScan(scanner: IScanner, doc: any) {
    scanner.scan(doc);
  }

  const oldPrinter = new OldCheapPrinter();
  // performScan(oldPrinter, {}); // This is now a COMPILE-TIME error!
  ```

- **The Improvement:**
  - **Safety:** Compile-time errors replace runtime crashes.
  - **High Cohesion:** Each interface represents a single, clear capability.
  - **Flexibility:** It's now easy to create new combinations of functionality.

## 🤔 **Reflective Questions**

1. How is the Interface Segregation Principle related to the Single Responsibility Principle?
2. In dynamically typed languages (like JS), how can the _spirit_ of ISP still be applied?
3. Can you identify an example of a "fat" class or a well-segregated set of interfaces in a framework you've used?

## 📚 **Further Reading**

- **The Bible:** Robert C. Martin's books provide the canonical explanation.
- **API Design:** ISP is a cornerstone of good API design.
- **SOLID Principles:** A great overview article: [The I in SOLID](https://www.google.com/search?q=https://www.baeldung.com/solid-principles%234-interface-segregation-principle)

## 📝 **Mini Task (Production)**

You are given a fat `IRepository` interface for a data layer. It includes CRUD and search. Some clients only need to _read_ data.

**Your Mission:**
Refactor the `IRepository` into smaller interfaces (e.g., `IReader`, `IWriter`, `ISearcher`).

```ts
interface User {
  id: string;
  name: string;
}

interface IRepository<T> {
  getById(id: string): T | undefined;
  getAll(): T[];
  search(query: string): T[];
  create(item: T): void;
  update(id: string, item: T): void;
  delete(id: string): void;
}
```
