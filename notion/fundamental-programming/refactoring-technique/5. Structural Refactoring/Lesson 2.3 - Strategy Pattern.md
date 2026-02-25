# 💡 SR02.3: Strategy Pattern

**Outline:**

- **The Smell (SEEI):** A conditional that switches between different algorithms or business rules.
- **The Refactor (PPP):** Applying the **Strategy Design Pattern** as a formal, step-by-step method for replacing the conditional with polymorphism.
- **Your Refactoring Mission (Production):** Time to convert a validation `switch` into a full Strategy pattern.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Smell (SEEI)**

**The Code Smell: Algorithm-Switching Conditional**
This is a specific variant of "Conditional Hell." It occurs when an `if-else` chain or `switch` switches between different **algorithms**, **business rules**, or **strategies** to achieve the same goal.

**The Negative Impact**

- **High Maintenance:** The class hosting the `switch` becomes bloated with many complex, unrelated algorithms.
- **Difficult Testing:** You must write complex tests for the container class just to verify a single specialized algorithm.
- **No Reusability:** An algorithm trapped in a `case` block cannot be used anywhere else in the system.
- **Coupling:** The main class is tightly coupled to every single specific implementation.

**Example of the Smell**
A calculator that "switches" discount types manually.

```ts
public getFinalTotal(type: DiscountType): number {
  switch (type) {
    case 'PERCENTAGE': return this.total * 0.9;
    case 'FIXED_AMOUNT': return this.total - 5;
    default: return this.total;
  }
}
```

### **Part 2: Practice - The Refactor (PPP)**

**The Technique: Refactor to Strategy Pattern**
Strategy lets you define a family of algorithms, put each into a separate class, and make them interchangeable.

1. **The Strategy (Interface):** Defines the signature of the algorithm (e.g., `applyDiscount(total)`).
2. **Concrete Strategies (Classes):** Individual classes for each algorithm.
3. **The Context (Original Class):** Holds a reference to a Strategy object and delegates the work instead of switching.

**Step-by-Step Implementation**

1. Create `IDiscountStrategy`.
2. Move switch logic into `PercentageDiscount` and `FixedAmountDiscount` classes.
3. Update `Invoice` to hold an instance of `IDiscountStrategy`.
4. The client now injects the strategy at runtime.

## 🧠 **Real-World Case Study: "Before vs. After Refactoring"**

- **Before (Algorithm-Switching):**
  An `Invoice` class that must be modified every time marketing invents a new discount rule.
- **After (Strategy Pattern):**

  ```ts
  interface Strategy {
    apply(total: number): number;
  }
  class PercentageDiscount implements Strategy {
    apply(t) {
      return t * 0.9;
    }
  }

  class Invoice {
    constructor(private strategy: Strategy) {}
    public getFinal() {
      return this.strategy.apply(this.total);
    }
  }
  ```

- **The Improvement:** `Invoice` no longer cares _how_ discounts work. To add a "Buy One Get One" discount, you just create a new class. `Invoice` remains untouched, fulfilling the Open/Closed Principle.

## 🤔 **Reflective Questions**

1. How does Strategy help with the Single Responsibility Principle?
2. What are the trade-offs of the client choosing the strategy vs. the class itself?
3. What is the difference between the **State** pattern and the **Strategy** pattern?

## 📚 **Further Reading**

- **Design Patterns (GoF):** The definitive source on Strategy.
- **Refactoring.Guru:** [Strategy Pattern](https://refactoring.guru/design-patterns/strategy).
- **Head First Design Patterns:** Approaches patterns with visual simplicity.

## 📝 **Mini Task (Production)**

Refactor this `Validator` class into a full Strategy pattern.

```ts
class Validator {
  public validate(input: string, type: "EMAIL" | "PASSWORD"): boolean {
    switch (type) {
      case "EMAIL":
        return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(input);
      case "PASSWORD":
        return input.length >= 8;
    }
  }
}
```

**Mission:**

1. Create `IValidationStrategy`.
2. Implement `EmailValidation` and `PasswordValidation`.
3. Update `Validator` to delegate to a strategy.
4. Show how the client would use it.
