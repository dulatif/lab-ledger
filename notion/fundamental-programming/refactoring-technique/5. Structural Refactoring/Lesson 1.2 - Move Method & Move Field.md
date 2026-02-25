# 💡 SR01.2: Move Method & Move Field

**Outline:**

- **The Smell (SEEI):** Identifying "Feature Envy," where a method is more interested in another class's data.
- **The Refactor (PPP):** Applying "Move Method" and "Move Field" to improve cohesion.
- **Your Refactoring Mission (Production):** Time to relocate some envious code.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Smell (SEEI)**

**The Code Smell: Feature Envy**
This occurs when a method in one class spends most of its time calling methods or accessing data on another class. The method is "envious" of the other class's data. The core principle is: **Code should be close to the data it operates on.**

**The Negative Impact**

- **High Coupling:** The envious class depends on the internal details of the target class. Changes in the target class break the envious one.
- **Low Cohesion:** The method doesn't belong in its current class, making that class less focused.
- **Violates "Tell, Don't Ask":** Envious methods often "ask" for data via getters and make decisions themselves. Good design "tells" the object to do the work.

**Example of the Smell**
A `calculateInterest` function that "asks" an `Account` object for multiple pieces of data to perform math.

```ts
// SMELL: This method is envious of the Account class.
function calculateInterest(account: Account): number {
  if (account.getType().isPremium()) return 10.5;
  if (account.getDaysOverdrawn() > 7) return 30.0;
  return 20.0;
}
```

### **Part 2: Practice - The Refactor (PPP)**

**The Technique: Move Method & Move Field**
Relocate the method to the class it envies. If the method uses a field that only it needs, move the field too.

1. **Declare** the method in the target class.
2. **Copy and Adjust:** Paste code into the new class. Change data access (e.g., `account.getDaysOverdrawn()` -> `this.daysOverdrawn`).
3. **Delegate or Replace:** Change the original call to use the new method on the target class.
4. **Clean Up:** Delete the original method once all callers are updated.

**Step-by-Step Implementation**

1. Move interest calculation logic to `Account.calculateInterest()`.
2. `Account` now uses its private fields directly.
3. Client code calls `myAccount.calculateInterest()` instead of a separate utility function.

## 🧠 **Real-World Case Study: "Before vs. After Refactoring"**

- **Before (Feature Envy):**
  A utility class that "reaches into" a domain object's internal state. Every minor domain change breaks the utility.
- **After (Move Method):**
  ```ts
  class Account {
    // Logic is now where it belongs
    public calculateInterest() {
      if (this.type.isPremium()) return 10.5;
      return this.daysOverdrawn > 7 ? 30.0 : 20.0;
    }
  }
  ```
- **The Improvement:** Higher cohesion. `Account` encapsulates both its data and its behavioral rules. Coupling is reduced; external code no longer needs to know how interest is calculated.

## 🤔 **Reflective Questions**

1. How does "Tell, Don't Ask" solve Feature Envy?
2. What is the risk of manual "Move Method" vs. using an IDE tool?
3. When should you _not_ move a method that uses another class's data? (e.g., Strategy/Visitor patterns).

## 📚 **Further Reading**

- **The Bible:** Martin Fowler's "Refactoring".
- **Refactoring.Guru:** [Move Method](https://refactoring.guru/move-method).
- **Concept:** [Tell, Don't Ask](https://martinfowler.com/bliki/TellDontAsk.html).

## 📝 **Mini Task (Production)**

Refactor `Order.isEligibleForFreeShipping`. It currently only cares about `Customer` data. Move it to `Customer`.

```ts
class Order {
  // SMELL: Feature Envy
  public isEligibleForFreeShipping(): boolean {
    const isDomestic = this.customer.getAddress() !== "International";
    return this.customer.getIsPremiumMember() && isDomestic;
  }
}
```

**Mission:** Move the logic to `Customer.isEligibleForFreeShipping()` and update the `Order` class to delegate (or call it directly from the client).
