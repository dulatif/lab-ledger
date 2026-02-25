# 💡 BRT01.2: Inline Method

**Outline:**

- **The Smell (SEEI):** Identifying when a method is an unnecessary "Middle Man".
- **The Refactor (PPP):** Applying the "Inline Method" technique to simplify the code.
- **Your Refactoring Mission (Production):** Time to clean up some code.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Smell (SEEI)**

**The Code Smell: Middle Man / Unnecessary Abstraction**
This is the opposite of a Long Method. It occurs when a method does so little that its existence is questionable. Often, the body just delegates to another method without adding any descriptive value. This creates unnecessary indirection.

**The Negative Impact**

- **Cognitive Overhead:** Every function call forces a mental jump to its definition. If it's just another jump, it's wasted effort.
- **Navigation Clutter:** Too many methods make it harder to find the real logic.
- **Hides Work:** Simple delegations can obscure which object is actually responsible for the task.

**Example of the Smell**
A method that merely returns the result of another method without logic or clarity improvements.

```ts
class Order {
  // SMELL: Middle Man. Adds no value.
  public getFinalPrice(): number {
    return this.calculateDiscountedPrice();
  }

  private calculateDiscountedPrice(): number {
    return this.basePrice - this.discount;
  }
}
```

### **Part 2: Practice - The Refactor (PPP)**

**The Technique: Inline Method**
Used to eliminate unnecessary indirections.

1. **Find all callers** of the "Middle Man" method.
2. **Replace calls** with the body of the method itself.
3. **Delete** the original method.

**Step-by-Step Implementation**
In the `Order` example:

1. Replace `getFinalPrice()` calls.
2. Promoting the logic from `calculateDiscountedPrice` directly into a public `getFinalPrice`.
3. Removing the extra layer.

## 🧠 **Real-World Case Study: "Before vs. After Refactoring"**

- **Before (Middle Man):**
  A class with nested private methods that just pass data along.
- **After (Inline Method):**
  ```ts
  class Order {
    public getFinalPrice(): number {
      return this.basePrice - this.discount;
    }
  }
  ```
- **The Improvement:** Removed a layer of indirection. The code is more direct, easier to navigate, and has fewer functions to maintain.

## 🤔 **Reflective Questions**

1. When is a small function a _good_ thing vs. just added complexity?
2. How does this technique relate to the YAGNI principle?
3. What is the main risk of over-using Inline Method?

## 📚 **Further Reading**

- **The Bible:** Martin Fowler's "Refactoring".
- **Refactoring.Guru:** [Inline Method](https://refactoring.guru/inline-method).
- **Principles:** YAGNI (You Ain't Gonna Need It).

## 📝 **Mini Task (Production)**

Refactor `ReportGenerator` by inlining the unnecessary `createHeader` and `createBody` wrappers.

```ts
class ReportGenerator {
  public generate(): string {
    const header = this.createHeader(); // INLINE THIS
    const body = this.createBody(); // INLINE THIS
    return header + body;
  }

  private createHeader(): string {
    return this.formatHeader();
  }

  private createBody(): string {
    return this.formatBody();
  }

  private formatHeader() {
    return "--- HEADER ---\n";
  }
  private formatBody() {
    return this.data.join("\n");
  }
}
```
