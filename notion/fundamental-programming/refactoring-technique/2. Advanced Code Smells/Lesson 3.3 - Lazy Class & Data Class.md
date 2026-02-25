# 💡 ACS03.3: Lazy Class & Data Class

**Outline:**

- **The Smell (SEEI):** Identifying `Lazy Class` (does nothing) and `Data Class` (has no behavior) as signs of missing or misplaced responsibility.
- **The Refactor (PPP):** Applying "Inline Class" to remove useless classes and "Move Method" to enrich data classes.
- **Your Refactoring Mission (Production):** Time to deal with a lazy class.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Smell (SEEI)**

**The Code Smell: Lazy Class & Data Class**
These are two sides of the same coin: a class that isn't pulling its weight.

- **Lazy Class:** A class that does almost nothing after a refactoring. It might just delegate a call or have one simple method. It adds maintenance overhead for very little value.
- **Data Class:** A class containing only fields and getters/setters. It has no behavior of its own. While useful for DTOs, it often indicates "Anemic Domain Model" where behavior is scattered elsewhere (Feature Envy).

**The Negative Impact: Unnecessary Complexity and Anemic Models**

- **Cognitive Overhead:** Every class is a concept a developer must learn. Lazy classes are pure waste.
- **Anemic Domain Model:** Leads to procedural style where "Service" classes have all logic while objects are just dumb buckets of data.
- **Missed Encapsulation:** Logic outside the data class forces it to expose internal state.

**Example of the Smell: Lazy Class and Data Class**

```ts
// LAZY CLASS: Logic could be a single line.
class PhoneNumberFormatter {
  private phone: Phone;
  constructor(phone: Phone) {
    this.phone = phone;
  }

  public getFormattedNumber(): string {
    return `(${this.phone.areaCode}) ${this.phone.prefix}-${this.phone.lineNumber}`;
  }
}

// DATA CLASS: Data but zero behavior.
class Rectangle {
  public width: number;
  public height: number;
}
// Behavior is somewhere else (Feature Envy)
function calculateArea(rect: Rectangle): number {
  return rect.width * rect.height;
}
```

### **Part 2: Practice - The Refactor (PPP)**

**The Technique: Inline Class & Move Method**

- **Lazy Class:** Use **Inline Class**. Move functionality into the calling class and delete the lazy class.
- **Data Class:** Use **Move Method**. Move external methods that operate on the data class _into_ the data class.

**Step-by-Step Implementation: Fixing the Smells**

```ts
// FIXING THE DATA CLASS:
class Rectangle {
  public width: number;
  public height: number;

  constructor(width: number, height: number) {
    this.width = width;
    this.height = height;
  }

  // Move behavior INTO the class with the data.
  public getArea(): number {
    return this.width * this.height;
  }
}
// Client code is now object-oriented:
const rect = new Rectangle(10, 5);
const area = rect.getArea();
```

## 🧠 **Real-World Case Study: "Before vs. After Refactoring"**

- **Before (Data Class):**
  ```ts
  class DateRange {
    public start: Date;
    public end: Date;
  }
  function isInRange(range: DateRange, date: Date): boolean {
    return date >= range.start && date <= range.end;
  }
  ```
- **After (Rich Object):**
  ```ts
  class DateRange {
    private start: Date;
    private end: Date;
    public contains(date: Date): boolean {
      return date >= this.start && date <= this.end;
    }
  }
  ```
- **The Improvement:** `DateRange` is now a true object that understands its own behavior. Logic is encapsulated ("Tell, Don't Ask"), and client code is cleaner: `myRange.contains(aDate)`.

## 🤔 **Reflective Questions**

1. When is a Data Class (like a DTO) perfectly acceptable?
2. How do you find the right balance for class responsibility to avoid both God Objects and Lazy Classes?
3. How does moving behavior into Data Classes reduce system-wide Feature Envy?

## 📚 **Further Reading**

- **The Bible:** Fowler's "Refactoring" - "Inline Class" and "Move Method."
- **Refactoring.guru:** [Lazy Class](https://refactoring.guru/smells/lazy-class) and [Data Class](https://refactoring.guru/smells/data-class).
- **Anemic Domain Model:** Martin Fowler’s essay on this common antipattern.

## 📝 **Mini Task (Production)**

Refactor the `Price` and `PriceCalculator` code below to create a rich `Price` object.

**Your Mission:**

1. Move calculation methods from `PriceCalculator` into the `Price` class.
2. Delete the now-empty `PriceCalculator` (Lazy Class).
3. Update client code.

```ts
class Price {
  public basePrice: number;
  public taxRate: number;
  public discountRate: number;
}

class PriceCalculator {
  public getFinalPrice(price: Price): number {
    return price.basePrice * (1 + price.taxRate);
  }
  public getDiscountedPrice(price: Price): number {
    const finalPrice = this.getFinalPrice(price);
    return finalPrice * (1 - price.discountRate);
  }
}
```
