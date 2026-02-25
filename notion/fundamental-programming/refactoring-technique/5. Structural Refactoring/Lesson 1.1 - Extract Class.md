# 💡 SR01.1: Extract Class

**Outline:**

- **The Smell (SEEI):** Identifying the "God Class" or "Large Class" that violates the Single Responsibility Principle.
- **The Refactor (PPP):** Applying the "Extract Class" technique to create smaller, more cohesive classes.
- **Your Refactoring Mission (Production):** Time to decompose a bloated class.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Smell (SEEI)**

**The Code Smell: Large Class (or God Class)**
This occurs when a class has taken on too many responsibilities over time. It knows too much and does too much. A key sign is groups of instance variables only used by a subset of methods (e.g., address fields vs. authentication fields in a `User` class).

**The Negative Impact**

- **Violates SRP (Single Responsibility Principle):** The class has multiple reasons to change.
- **Low Cohesion:** Class elements don't work together toward a single purpose.
- **High Coupling:** Clients needing one responsibility must depend on the whole bloated class.
- **Blocks Reuse:** You can't take the address logic without the entire `User` object.

**Example of the Smell**
A `Person` class that manages personal details AND full telephone number logic.

```ts
class Person {
  private name: string;
  private officeAreaCode: string; // Phone logic
  private officeNumber: string; // Phone logic

  public getTelephoneNumber() {
    return `(${this.officeAreaCode}) ${this.officeNumber}`;
  }
}
```

### **Part 2: Practice - The Refactor (PPP)**

**The Technique: Extract Class**
Create a new class and move the relevant fields and methods into it. The original class then uses composition.

1. **Create** the new class (e.g., `TelephoneNumber`).
2. **Move** relevant fields and methods (`areaCode`, `toString`).
3. **Use Composition:** Give `Person` a field of type `TelephoneNumber`.
4. **Delegate:** Methods in `Person` now call the new class.

**Step-by-Step Implementation**

1. Move `officeAreaCode` and `officeNumber` to a new `TelephoneNumber` class.
2. `Person` now has `private telephone: TelephoneNumber`.
3. `Person.getTelephoneNumber()` becomes `return this.telephone.toString()`.

## 🧠 **Real-World Case Study: "Before vs. After Refactoring"**

- **Before (Low Cohesion):**
  One class managing two distinct domain concepts (Person and Phone).
- **After (Extract Class):**

  ```ts
  class TelephoneNumber {
    constructor(
      private areaCode: string,
      private number: string,
    ) {}
    public toString() {
      return `(${this.areaCode}) ${this.number}`;
    }
  }

  class Person {
    private telephone: TelephoneNumber; // Composition
    // ...
    public getTelephoneNumber() {
      return this.telephone.toString();
    }
  }
  ```

- **The Improvement:** Two highly cohesive classes. `TelephoneNumber` is now reusable elsewhere. `Person` is simpler and adheres to SRP.

## 🤔 **Reflective Questions**

1. What is SRP, and how does a "God Class" violate it?
2. How does `Extract Class` improve testability?
3. What naming patterns (prefixes) suggest a candidate for extraction?

## 📚 **Further Reading**

- **The Bible:** Martin Fowler's "Refactoring".
- **Refactoring.Guru:** [Extract Class](https://refactoring.guru/extract-class).
- **SOLID:** Single Responsibility Principle.

## 📝 **Mini Task (Production)**

Refactor this `Order` class by extracting a `ShippingCalculator` and an `InvoiceGenerator`.

```ts
class Order {
  // Responsibility 1: Calculation
  public getTotal() {
    /* ... */
  }

  // Responsibility 2: Shipping (EXTRACT THIS)
  public getShippingCost() {
    const totalWeight = this.items.reduce((s, i) => s + i.weight, 0);
    return this.method === "GROUND" ? totalWeight * 1.5 : totalWeight * 3;
  }

  // Responsibility 3: Invoicing (EXTRACT THIS)
  public getInvoice() {
    return `Items: $${this.getTotal()}\nShipping: $${this.getShippingCost()}`;
  }
}
```

**Goal:** Delegate shipping to `ShippingCalculator` and string formatting to `InvoiceGenerator`.
