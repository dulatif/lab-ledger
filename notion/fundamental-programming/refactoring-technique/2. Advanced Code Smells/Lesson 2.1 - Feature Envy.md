# 💡 ACS02.1: Feature Envy

**Outline:**

- **The Smell (SEEI):** Identifying "Feature Envy," where a method is more interested in another class's data than its own.
- **The Refactor (PPP):** Applying the "Move Method" technique to place behavior with the data it operates on.
- **Your Refactoring Mission (Production):** Time to relocate an envious method.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Smell (SEEI)**

**The Code Smell: Feature Envy**
This occurs when a method in one class spends most of its time calling getters or accessing data from another class. The method is more "interested" in the other class than its own. It's a clear sign the method is in the wrong place; operations on data should live with the data itself.

**The Negative Impact: Poor Encapsulation and High Coupling**

- **Breaks Encapsulation:** Data classes often expose internal state via numerous getters just to satisfy the envious class.
- **High Coupling:** The two classes are tightly coupled; changing the data structure will break the envious method.
- **Duplicated Logic:** If multiple classes are envious of the same data, you'll often find duplicated logic.

**Example of the Smell: The `Billing` class**
The `printBill()` method in `Billing` is envious of the `Order` class.

```ts
class OrderItem {
  constructor(
    public name: string,
    public price: number,
    public quantity: number,
  ) {}
  public getTotalPrice(): number {
    return this.price * this.quantity;
  }
}

class Order {
  constructor(
    public customerName: string,
    private items: OrderItem[],
  ) {}
  public getItems(): OrderItem[] {
    return this.items;
  } // Exposing internal data
}

class Billing {
  public printBill(order: Order): string {
    let total = 0;
    for (const item of order.getItems()) {
      total += item.getTotalPrice();
    }
    return `Customer: ${order.customerName}, Total: $${total}`;
  }
}
```

The logic to calculate total should be inside the `Order` class.

### **Part 2: Practice - The Refactor (PPP)**

**The Technique: Move Method**
The solution is to move the method to the class that contains the data it uses.

1. **Identify the Envious Method.**
2. **Move the Method:** Copy the method into the class it envies.
3. **Adjust the Logic:** Use direct access to data (`this.`) and remove getter calls.
4. **Delegate or Remove:** In the original class, delegate the call or remove the old method.

**Step-by-Step Implementation: Moving `printBill`**
Move the calculation logic to `Order`.

```ts
class Order {
  constructor(
    public customerName: string,
    private items: OrderItem[],
  ) {}

  public calculateTotal(): number {
    let total = 0;
    for (const item of this.items) {
      total += item.getTotalPrice();
    }
    return total;
  }
}

class Billing {
  public printBill(order: Order): string {
    const total = order.calculateTotal(); // Simple delegation
    return `Customer: ${order.customerName}, Total: $${total}`;
  }
}
```

## 🧠 **Real-World Case Study: "Before vs. After Refactoring"**

- **Before (Feature Envy):**
  ```ts
  class Billing {
    public printBill(order: Order): string {
      let total = 0;
      for (const item of order.getItems()) {
        total += item.getPrice() * item.getQuantity();
      }
      return `Total: $${total}`;
    }
  }
  ```
- **After (Behavior Moved to Data):**
  ```ts
  class Order {
    public calculateTotal(): number {
      /* logic here */
    }
  }
  class Billing {
    public printBill(order: Order): string {
      const total = order.calculateTotal(); // "Tell, Don't Ask"
      return `Total: $${total}`;
    }
  }
  ```
- **The Improvement:**
  - **Better Encapsulation:** `Order` no longer needs to expose its internal items.
  - **Lower Coupling:** `Billing` doesn't care about the internal structure of `Order`.
  - **Single Source of Truth:** Total calculation logic lives in one place.

## 🤔 **Reflective Questions**

1. What is the "Tell, Don't Ask" principle?
2. What if you can't move a method (e.g., third-party library)?
3. How can Feature Envy reveal flaws in your domain model?

## 📚 **Further Reading**

- **The Bible:** Martin Fowler's "Refactoring" - "Move Method" technique.
- **Refactoring.guru:** Excellent [Feature Envy examples](https://refactoring.guru/smells/feature-envy).
- **SOLID Principles:** Feature Envy often leads to low cohesion and violates SRP.

## 📝 **Mini Task (Production)**

Refactor the `Customer` class below to eliminate Feature Envy of the `Phone` class.

**Your Mission:**

1. Move `getFormattedPhoneNumber` logic to the `Phone` class.
2. Delegate the call from `Customer`.

```ts
class Phone {
  public areaCode: string;
  public prefix: string;
  public lineNumber: string;

  constructor(area: string, prefix: string, line: string) {
    this.areaCode = area;
    this.prefix = prefix;
    this.lineNumber = line;
  }
}

class Customer {
  public name: string;
  public phone: Phone;

  constructor(name: string, phone: Phone) {
    this.name = name;
    this.phone = phone;
  }

  public getFormattedPhoneNumber(): string {
    return `(${this.phone.areaCode}) ${this.phone.prefix}-${this.phone.lineNumber}`;
  }
}
```
