# 14. Abstract Classes and Method Overriding

## 🎯 Learning Goal

Define base templates for classes.

## 🧠 Concept

**Abstract Class**: Cannot be instantiated. Used as a base.
**Abstract Method**: Must be implemented by derivation.

## 💻 Implementation

```typescript
abstract class PaymentProcessor {
  abstract process(amount: number): void; // No implementation

  log(msg: string) {
    console.log(msg); // Shared implementation
  }
}

class Stripe extends PaymentProcessor {
  process(amount: number) {
    this.log(`Stripe: ${amount}`);
  }
}

// const p = new PaymentProcessor(); // Error
```

## 🧩 Activity / Challenge

1.  Create abstract `Document` with abstract `save()`.
2.  Extend `PDFDocument` and `HTMLDocument`.

## 🔑 Key Takeaways

- Great for the "Template Method" design pattern.
