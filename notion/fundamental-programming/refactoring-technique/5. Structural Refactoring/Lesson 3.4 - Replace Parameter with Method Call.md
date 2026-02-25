# 💡 SR03.4: Replace Parameter with Method Call

**Outline:**

- **The Smell (SEEI):** A parameter's value can be obtained by the method's own object.
- **The Refactor (PPP):** Applying "Replace Parameter with Method Call" to simplify signatures and increase encapsulation.
- **Your Refactoring Mission (Production):** Time to remove a redundant parameter.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Smell (SEEI)**

**The Code Smell: Redundant Parameter**
A caller calculates a value and passes it as a parameter, but the receiver object already has access to all the information needed to calculate that value itself. The caller is doing unnecessary work that the receiver is perfectly capable of.

**The Negative Impact**

- **Breaks Encapsulation:** The responsibility for calculating an internal concern is leaked to the caller.
- **Code Duplication:** Multiple callers might repeat the same calculation logic before calling the method.
- **Complex Signatures:** Methods become harder to understand and use because they require parameters that should be handled internally.

**Example of the Smell**
An `Order` class that asks for a `discountRate` even though it already has access to the `Customer` object.

```ts
// SMELL: Why pass rate? The order knows the customer.
public getFinalPrice(discountRate: number) {
  return this.basePrice * (1 - discountRate);
}

// Client Calculate & Pass
const rate = customer.getRate();
const price = order.getFinalPrice(rate);
```

### **Part 2: Practice - The Refactor (PPP)**

**The Technique: Replace Parameter with Method Call**
Move the logic from the caller into the receiver method.

1. **Verify Access:** Ensure the receiver (Order) has access to the data source (Customer).
2. **Move Logic:** Inside the receiver, call the method that provides the value.
3. **Clean Signature:** Remove the redundant parameter from the method definition and all call sites.

**Step-by-Step Implementation**

1. Update `getFinalPrice()` to take zero arguments.
2. Inside the method, add `const rate = this.customer.getDiscountRate();`.
3. Update callers: `order.getFinalPrice();`.

## 🧠 **Real-World Case Study: "Before vs. After Refactoring"**

- **Before (Redundant Parameter):**
  The client has to "pre-calculate" everything before calling any order methods. Changing how discounts are calculated might require a multi-file sweep.
- **After (Encapsulated Call):**
  ```ts
  class Order {
    public getFinalPrice() {
      const rate = this.customer.getDiscountRate();
      return this.basePrice * (1 - rate);
    }
  }
  ```
- **The Improvement:** Better encapsulation. The client no longer needs to know how discount rates are fetched or calculated. The `Order` class becomes a self-sufficient expert on its own pricing logic.

## 🤔 **Reflective Questions**

1. How does this refactoring improve the "expert" status of the receiver class?
2. When might this backfire? (Hint: if you couple the method too tightly to a complex external object).
3. How is this different from "Introduce Parameter Object"?

## 📚 **Further Reading**

- **The Bible:** Martin Fowler's "Refactoring".
- **Refactoring.Guru:** [Replace Parameter with Method Call](https://refactoring.guru/replace-parameter-with-method-call).

## 📝 **Mini Task (Production)**

Refactor `ReportGenerator.generate`. It takes a `title` even though it has a `config` object containing that title.

```ts
class ReportGenerator {
  public generate(title: string, data: any[]) {
    // ...
  }
}
```

**Mission:**

1. Remove the `title` parameter.
2. Fetch the title from `this.config.getTitle()` inside the method.
3. Update the client code call to `generator.generate(data)`.
