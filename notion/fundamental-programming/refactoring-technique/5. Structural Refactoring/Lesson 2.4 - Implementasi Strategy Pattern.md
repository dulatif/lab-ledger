# 💡 SR02.4: Implementasi Strategy Pattern

**Outline:**

- **The Smell (SEEI):** A quick review of "Conditional Hell" as our target.
- **The Refactor (PPP):** A practical guide to implementing polymorphic solutions (like Strategy) using core TypeScript features.
- **Your Refactoring Mission (Production):** Time to build a complete, production-ready polymorphic solution.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Smell (SEEI)**

**The Code Smell: The "Type Code" String/Enum**
The trigger for `switch` statements is often a "type code" (string, number, or enum). This "stringly-typed" approach is fragile. A simple typo (`'Fedex'` vs `'FEDEX'`) can cause bugs that the compiler cannot catch. Our goal is to move from passing primitive codes to passing rich, capable **objects** that encapsulate behavior.

**The Negative Impact**

- **Primitive Obsession:** Using a simple string to represent complex business logic.
- **No Type Safety:** Compilers can't detect typos in magic strings.
- **Decoupled Data and Behavior:** The type code is in one file, while the logic that operates on it is trapped in a global `switch` block.

**Example of the Smell**
An exporter function that depends on magic string formats.

```ts
function exportData(data: any, format: "JSON" | "CSV") {
  switch (format) {
    case "JSON":
      return JSON.stringify(data);
    case "CSV":
      return "id,name\n1,test";
  }
}
```

### **Part 2: Practice - The Refactor (PPP)**

**The Technique: Implementing Polymorphism with TypeScript**
We combine `interface`, concrete `classes`, and the **Factory Pattern** to build a robust solution.

1. **`interface`:** Defines the contract (e.g., `IExporter { export(data) }`).
2. **`class ... implements Interface`:** Creates the interchangeable strategy objects.
3. **The Factory Pattern:** This is the ONLY place where a `switch` should exist. It translates the primitive "type code" into a powerful strategy object, isolating the conditional logic from the rest of the app.

**Step-by-Step Implementation Pattern**

1. **Define the Interface:** `interface IExporter { export(data: any[]): string; }`.
2. **Implement Classes:** `JsonExporter`, `CsvExporter`.
3. **Create the Factory:** A function `createExporter(format)` containing the isolated `switch`.
4. **Update Client:** The client asks the factory for an exporter and then calls `.export()`.

## 🧠 **Real-World Case Study: "Before vs. After Refactoring"**

- **Before (Fragile):**
  The client code is forced to know about every format string. Adding XML means changing the main export function.
- **After (Robust with Factory):**

  ```ts
  interface IExporter {
    export(data: any[]): string;
  }
  class JsonExporter implements IExporter {
    /* ... */
  }

  function createExporter(format: string): IExporter {
    if (format === "JSON") return new JsonExporter();
    // ...
  }

  const exporter = createExporter("JSON");
  const output = exporter.export(myData);
  ```

- **The Improvement:** The client is decoupled from the `switch`. The `switch` is trapped in the factory. Adding `XMLExporter` only requires a new class and one line in the factory. No other code changes.

## 🤔 **Reflective Questions**

1. Why is a Factory the best place for the "last `switch` statement"?
2. How do `interface` and `implements` enforce contracts at compile time?
3. How could **Dependency Injection** replace the need for a factory?

## 📚 **Further Reading**

- **GoF:** Factory Method and Abstract Factory patterns.
- **TS Handbook:** [Interfaces and Objects](https://www.typescriptlang.org/docs/handbook/2/objects.html).
- **Refactoring.Guru:** [Factory Method](https://refactoring.guru/design-patterns/factory-method).

## 📝 **Mini Task (Production)**

Implement a production-quality payment processing system:

1. Define `IPaymentGateway` with `pay(order: Order): boolean`.
2. Implement classes `CreditCardGateway`, `PayPalGateway`, and `CryptoGateway`.
3. Create a factory `createPaymentGateway(method: string): IPaymentGateway`.
4. Write client code that uses the factory for a PayPal transaction.

**Mission:** Ensure the `switch` is ONLY inside the factory and nowhere else.
