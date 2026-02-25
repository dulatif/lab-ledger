# 💡 AR03.2: Push Down Method & Push Down Field

**Outline:**

- **The Smell (SEEI):** A method or field in a superclass is only relevant to a subset of its children.
- **The Refactor (PPP):** Applying the "Push Down Method" and "Push Down Field" refactorings to move specialized code into the appropriate subclasses.
- **Your Refactoring Mission (Production):** Time to move irrelevant behavior out of a superclass.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Smell (SEEI)**

**The Code Smell: Irrelevant Behavior in Superclass**
This is the inverse of the "Duplication in Siblings" smell. It occurs when a superclass contains behavior or data that only some of its subclasses actually use. For example, if a base `Employee` class has a `getCommission()` method, it's likely only relevant for `Salesperson`, not for `Engineer`.

**The Negative Impact**

- **Violates LSP:** Clients seeing a generic `Employee` might call `getCommission()` on an `Engineer`, leading to awkward 0-returns or runtime errors.
- **Leaky Abstraction:** The superclass loses its "general purpose" status by carrying around specific details that only apply to a niche group of children.
- **Confusing API:** It pollutes the public interface of unrelated subclasses with noise.

**Example of the Smell**
A `BillingPlan` superclass with a `getOverageRate()` method that is only needed by its `MeteredPlan` subclass, but is inherited by `FixedPlan` which has no concept of overages.

### **Part 2: Practice - The Refactor (PPP)**

**The Technique: Push Down Method / Push Down Field**
Moving specialized functionality from a superclass down into the specific subclasses where it belongs.

1. **Identify the Outlier:** Find the method/field used by only one or two specific children.
2. **Move to Subclass:** Copy the code from the superclass to those specific children.
3. **Delete from Super:** Remove the original from the parent class.
4. **Clean up Clients:** If external code was calling this on the base type, you must now cast to the specific child or rethink the client's abstraction.

## 🧠 **Real-World Case Study: "The Specialized Billing Plan"**

- **Before (Leaky Superclass):**
  `BillingPlan` has `getOverageRate()`. Both `FixedPlan` and `MeteredPlan` inherit it, but it's only meaningful for `MeteredPlan`. `FixedPlan`'s API is cluttered with a useless method.
- **After (Clean Abstraction):**

  ```ts
  abstract class BillingPlan {
    public abstract calculateBill(usage: number): number;
  }

  class MeteredPlan extends BillingPlan {
    public getOverageRate() { return 0.10; }
    public calculateBill(usage: number) { ... uses rate ... }
  }
  ```

- **The Improvement:** Better cohesion. The `BillingPlan` is now a pure, general base. `FixedPlan` is no longer a "lying" object that claims to have an overage rate. The code reflects the true relationships of the domain.

## 🤔 **Reflective Questions**

1. How does this technique help satisfy the **Interface Segregation Principle**?
2. What does this reveal about your initial superclass design?
3. If a method is used by 90% of children, is it better to push it down or keep it in the parent?

## 📚 **Further Reading**

- **Refactoring:** Martin Fowler on Push Down.
- **Refactoring.Guru:** [Push Down Field](https://refactoring.guru/push-down-field) and [Push Down Method](https://refactoring.guru/push-down-method).

## 📝 **Mini Task (Production)**

Refactor the `Document` hierarchy.

```ts
class Document {
  public sign(signature: string) { ... }
}
```

**Mission:**

1. `Document` has a `sign()` method, but it only applies to `LegalDocument`. `BlogPost` should not have it.
2. Move the `sign()` method into `LegalDocument`.
3. Provide the final, clean `Document` base class.
