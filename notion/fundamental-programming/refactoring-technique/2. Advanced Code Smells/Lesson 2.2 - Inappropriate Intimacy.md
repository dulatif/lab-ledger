# 💡 ACS02.2: Inappropriate Intimacy

**Outline:**

- **The Smell (SEEI):** Identifying "Inappropriate Intimacy," where two classes are excessively coupled and depend on each other's private implementation details.
- **The Refactor (PPP):** Applying techniques like "Move Method/Field" and "Hide Delegate" to create clear boundaries.
- **Your Refactoring Mission (Production):** Time to separate a pair of overly intimate classes.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Smell (SEEI)**

**The Code Smell: Inappropriate Intimacy**
This occurs when two classes are tightly coupled, often reaching into each other's private or protected members. They behave like a couple who knows too much about each other's personal lives. This is a severe violation of encapsulation and creates a fragile link; if one class changes its internal implementation, the other breaks.

**The Negative Impact: The Tangled Web**

- **Extreme Coupling:** You cannot change one class without understanding and likely changing the other.
- **Hidden Dependencies:** Dependencies are on private implementation details, which should be free to change.
- **Brittleness:** Seemingly safe refactorings (like renaming a private field) can cause unexpected breaks.
- **No Clear Boundaries:** Responsibilities are intertwined, making it hard to determine what each class does.

**Example of the Smell: The `Order` and `OrderDiscount` classes**
The `OrderDiscount` class reaches directly into the `Order` class's private-by-convention `_items` array.

```ts
class OrderItem {
  constructor(
    public price: number,
    public quantity: number,
  ) {}
}

class Order {
  public _items: OrderItem[] = []; // Supposed to be private!
  public customer: Customer;

  public addItem(item: OrderItem) {
    this._items.push(item);
  }
}

class OrderDiscount {
  public calculateDiscount(order: Order): number {
    let discount = 0;
    // Reaches directly into Order's internal state
    if (order._items.length > 5) {
      discount = 0.1;
    }
    if (order.customer.isPremium()) {
      discount += 0.05;
    }
    return discount;
  }
}
```

### **Part 2: Practice - The Refactor (PPP)**

**The Technique: Establish Clear Boundaries**
Make classes communicate only through their public APIs.

1. **Identify the Intimacy:** Pinpoint where internal details are being leaked.
2. **Move Method/Field:** Move logic that belongs to the other class.
3. **Create Public Methods:** Expose high-level methods that provide info without revealing structure.
4. **Hide Delegate:** If navigating through objects (like `order.customer.isPremium()`), the main class should provide a method to hide that navigation.

**Step-by-Step Implementation: Separating `Order` and `OrderDiscount`**
Provide clean methods on `Order`.

```ts
class Order {
  private _items: OrderItem[] = []; // Now truly private
  private customer: Customer;

  public getItemCount(): number {
    return this._items.length;
  }

  public isFromPremiumCustomer(): boolean {
    return this.customer.isPremium();
  }
}

class OrderDiscount {
  public calculateDiscount(order: Order): number {
    let discount = 0;
    // Calls public methods; doesn't know about `_items` or internal structure
    if (order.getItemCount() > 5) {
      discount = 0.1;
    }
    if (order.isFromPremiumCustomer()) {
      discount += 0.05;
    }
    return discount;
  }
}
```

## 🧠 **Real-World Case Study: "Before vs. After Refactoring"**

- **Before (Intimate):**
  ```ts
  class Permissions {
    public canEdit(user: User): boolean {
      return user._sessionData.get("role") === "admin"; // Reaches into session map
    }
  }
  ```
- **After (Decoupled):**
  ```ts
  class User {
    private _sessionData: Map<string, any>;
    public getRole(): string {
      return this._sessionData.get("role");
    }
  }
  class Permissions {
    public canEdit(user: User): boolean {
      return user.getRole() === "admin"; // Uses public API
    }
  }
  ```
- **The Improvement:**
  - **Encapsulation:** Internal decision to use a `Map` is hidden.
  - **Stability:** Public interface is a stable contract.
  - **Clearer Responsibilities:** `User` manages its session; `Permissions` checks roles.

## 🤔 **Reflective Questions**

1. How is Inappropriate Intimacy different from Feature Envy?
2. What is the Law of Demeter?
3. Why can inheritance be a strong form of intimacy?

## 📚 **Further Reading**

- **The Bible:** Fowler's "Refactoring" - "Hide Delegate" and "Move Field."
- **Refactoring.guru:** [Inappropriate Intimacy examples](https://refactoring.guru/smells/inappropriate-intimacy).
- **Law of Demeter:** Theoretical foundation for avoiding this smell.

## 📝 **Mini Task (Production)**

Refactor the `Motor` and `Car` classes below to remove Inappropriate Intimacy.

**Your Mission:**

1. Make `Motor.rpm` private.
2. Create public methods `accelerate()` and `decelerate()` on `Motor`.
3. Use those methods in `Car`.

```ts
class Motor {
  public rpm: number = 0;
  public turnOn() {
    this.rpm = 800;
  }
}

class Car {
  private motor: Motor;

  constructor() {
    this.motor = new Motor();
    this.motor.turnOn();
  }

  public pressGasPedal() {
    console.log("Increasing RPM...");
    this.motor.rpm += 500;
  }

  public pressBrake() {
    console.log("Decreasing RPM...");
    this.motor.rpm -= 500;
  }
}
```
