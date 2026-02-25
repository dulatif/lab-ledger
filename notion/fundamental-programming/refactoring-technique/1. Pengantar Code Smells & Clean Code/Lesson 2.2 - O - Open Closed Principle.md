# 💡 SOLID02.2: O - The Open-Closed Principle

**Outline:**

- **The Smell (SEEI):** Identifying "Modification Mania" via rigid `switch` or `if/else` statements.
- **The Refactor (PPP):** Applying the Open/Closed Principle (OCP) to allow extension without modification.
- **Your Refactoring Mission (Production):** Time to make a class open to new behaviors.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Smell (SEEI)**

**The Code Smell: Modification Mania (The Rigid Switch Statement)**
This smell appears when you have a piece of code (often a `switch` statement or a long chain of `if/else if`) that you have to constantly _modify_ every time a new requirement comes in. For example, a function that calculates shipping costs might have a `switch` based on the shipping method. When the business adds a new "Drone Delivery" method, a programmer must go back and change the source code of that existing, tested function. This is risky.

**The Negative Impact: Why is modifying existing code bad?**
Every time you change existing, working code, you risk introducing bugs:

- **High Risk of Regression:** Changing the `calculateShipping` function could accidentally break the calculation for "Standard" shipping while you're adding "Drone" shipping.
- **Violates DRY:** The central `switch` statement often contains logic that could be better placed with the thing it's describing. The core module knows too much about all the different subtypes.
- **Becomes Unwieldy:** As more and more cases are added, the `switch` statement grows into a monster that is hard to read and maintain.

**Example of the Smell: The `OrderProcessor`**
This class must be modified every time a new payment method is introduced.

```ts
type PaymentMethod = "credit_card" | "paypal" | "crypto";

class OrderProcessor {
  public processPayment(amount: number, method: PaymentMethod) {
    switch (method) {
      case "credit_card":
        console.log(`Processing credit card payment for $${amount}...`);
        // ... logic for credit card gateway
        break;
      case "paypal":
        console.log(`Redirecting to PayPal for $${amount}...`);
        // ... logic for PayPal API
        break;
      case "crypto":
        console.log(`Processing crypto payment for $${amount}...`);
        // ... logic for crypto wallet
        break;
      default:
        throw new Error("Unsupported payment method");
    }
  }
}
```

To add a "Bank Transfer" payment method, you _must_ open this file and add another `case`. This violates the Open/Closed Principle.

### **Part 2: Practice - The Refactor (PPP)**

**The Technique: The Open/Closed Principle (OCP)**
The OCP states: **"Software entities (classes, modules, functions, etc.) should be open for extension, but closed for modification."**

This means you should be able to add new functionality without changing existing code. The primary way to achieve this is by relying on abstractions (like interfaces or abstract base classes) rather than concrete implementations. We will use a classic design pattern here: the **Strategy Pattern**.

**Step-by-Step Implementation: Refactoring the `OrderProcessor`**

1. **Create an Abstraction:** Define an interface that represents the concept of a "payment processor."
2. **Create Concrete Implementations:** For each payment method, create a separate class that implements this interface.
3. **Use the Abstraction:** Modify the `OrderProcessor` to depend on the interface, not the concrete types. It will receive a payment processor object and just use it, without knowing or caring what kind it is.

```ts
// Step 1: Create an abstraction (the "strategy" interface)
interface IPaymentProcessor {
  processPayment(amount: number): void;
}

// Step 2: Create concrete implementations
class CreditCardProcessor implements IPaymentProcessor {
  public processPayment(amount: number): void {
    console.log(`Processing credit card payment for $${amount}...`);
    // ... logic for credit card gateway
  }
}

class PayPalProcessor implements IPaymentProcessor {
  public processPayment(amount: number): void {
    console.log(`Redirecting to PayPal for $${amount}...`);
    // ... logic for PayPal API
  }
}

class CryptoProcessor implements IPaymentProcessor {
  public processPayment(amount: number): void {
    console.log(`Processing crypto payment for $${amount}...`);
    // ... logic for crypto wallet
  }
}

// Step 3: Use the abstraction.
class OrderProcessor {
  public processOrder(amount: number, processor: IPaymentProcessor) {
    // No more switch statement! It just calls the method on the provided object.
    processor.processPayment(amount);
  }
}
```

## 🧠 **Real-World Case Study: "Before vs. After Refactoring"**

- **Before (Rigid `switch`):**
  ```ts
  const orderProcessor = new OrderProcessor();
  orderProcessor.processPayment(100, "credit_card"); // Have to modify the class to add new types
  ```
- **After (Open via Abstraction):**

  ```ts
  const orderProcessor = new OrderProcessor();
  const creditCardProcessor = new CreditCardProcessor();
  orderProcessor.processOrder(100, creditCardProcessor);

  // To add a new payment method, we create a NEW class.
  class BankTransferProcessor implements IPaymentProcessor {
    public processPayment(amount: number): void {
      console.log(`Processing bank transfer for $${amount}...`);
    }
  }
  // And we use it without ever touching the OrderProcessor class!
  const bankTransferProcessor = new BankTransferProcessor();
  orderProcessor.processOrder(200, bankTransferProcessor); // It just works.
  ```

- **The Improvement:**
  - **Extensible:** We can add infinite new payment methods without ever changing `OrderProcessor.ts`.
  - **Stable:** The core `OrderProcessor` logic is **closed** for modification.
  - **Decoupled:** The `OrderProcessor` no longer needs to know about every single payment method.

## 🤔 **Reflective Questions**

1. OCP is often achieved using polymorphism. What is polymorphism and how does it enable this principle?
2. Can you think of a situation where a simple `switch` statement might actually be acceptable and refactoring to OCP would be over-engineering?
3. How does OCP support the Single Responsibility Principle?

## 📚 **Further Reading**

- **The Bible:** Robert C. Martin's "Agile Software Development, Principles, Patterns, and Practices" has the definitive chapter on OCP.
- **Design Patterns:** The Strategy Pattern is the key to this lesson. Learn more about it here: [Strategy Pattern - Refactoring Guru](https://refactoring.guru/design-patterns/strategy).
- **SOLID Principles:** A great overview article: [The O in SOLID](https://www.google.com/search?q=https://www.baeldung.com/solid-principles%232-openclosed-principle)

## 📝 **Mini Task (Production)**

You are given a `ReportExporter` class that can export a report into different formats. Currently, it uses an `if/else if` chain, violating the Open/Closed Principle.

**Your Mission:**
Refactor the `ReportExporter` to conform to the OCP. You should be able to add a new export format (e.g., XML) without modifying the `ReportExporter` class itself. Use the Strategy Pattern.

```ts
interface Report {
  title: string;
  content: string;
}

type ExportFormat = "JSON" | "CSV";

class ReportExporter {
  public export(report: Report, format: ExportFormat): string {
    if (format === "JSON") {
      console.log("Exporting to JSON...");
      return JSON.stringify(report, null, 2);
    } else if (format === "CSV") {
      console.log("Exporting to CSV...");
      return `${report.title},${report.content}`;
    } else {
      throw new Error("Unsupported format");
    }
  }
}
```
