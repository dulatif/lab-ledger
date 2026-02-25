# 💡 BRT03.4: Intro to "Conditional Hell"

**Outline:**

- **The Smell (SEEI):** Identifying "Conditional Hell" - when complex logic is the core problem.
- **The "Refactor" (PPP):** Introducing the concept of "Replace Conditional with Polymorphism" as the way out.
- **Your Refactoring Mission (Production):** A thought exercise on how to escape the hell.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Smell (SEEI)**

**The Code Smell: Conditional Hell (Large Switch Statements / `if-else` Chains)**
You've arrived at "Conditional Hell" when a function's primary job is to check an object's "type" and behave differently based on it. This manifests as giant `switch` statements or long `if-else-if` chains.

**The Negative Impact**

- **Fragile:** A change for one `case` can easily break another.
- **Hard to Maintain:** The function becomes a "god object" that knows too much about too many different types.
- **Violates SRP & OCP:** The function has many reasons to change (SRP). Every new type requires modifying the central conditional (OCP).
- **Blocks Scalability:** Adding new functionality becomes a high-risk manual chore.

**Example of the Smell**
A bird speed calculator where every new bird species requires a new `case`.

```ts
function getBirdSpeed(bird: Bird): number {
  switch (bird.type) {
    case "EUROPEAN":
      return 11;
    case "AFRICAN":
      return 60;
    case "PARROT":
      return bird.voltage / 10;
  }
}
```

### **Part 2: Practice - The Refactor (PPP)**

**The Technique: Intro to "Replace Conditional with Polymorphism"**
While Decompose or Guard Clauses clean up code, Polymorphism _eliminates_ the conditional entirely. Instead of asking "What type are you?", we tell the object: "Do your work."

**Conceptual Steps:**

1. **Identify** the type code (e.g., `bird.type`).
2. **Create Classes** for each type (e.g., `EuropeanSwallow`, `AfricanSwallow`).
3. **Common Interface:** Create an abstract base class/interface (e.g., `interface Bird { getSpeed(): number }`).
4. **Move Logic:** Move specific `case` logic into the concrete class methods.
5. **Call:** Replace the `switch` with `bird.getSpeed()`.

## 🧠 **Real-World Case Study: "Before vs. After (Conceptual)"**

- **Before (Conditional Hell):**
  A central `switch` that knows about every bird species. Adding a bird means editing this sensitive function.
- **After (Polymorphism):**

  ```ts
  abstract class Bird {
    abstract getSpeed(): number;
  }
  class EuropeanSwallow extends Bird {
    getSpeed() {
      return 11;
    }
  }

  const speed = myBird.getSpeed();
  ```

- **The Improvement:** The `switch` is gone. Adding a `MadagascarPenguin` simply means adding a new class. No existing code is touched. This is the cornerstone of robust, maintainable design.

## 🤔 **Reflective Questions**

1. How does Polymorphism support the Open/Closed Principle?
2. What are the risks of this larger refactoring, and how do tests help?
3. When is a `switch` statement actually acceptable?

## 📚 **Further Reading**

- **The Bible:** Martin Fowler's "Refactoring" (Replacing Conditionals).
- **Refactoring.Guru:** [Polymorphism](https://refactoring.guru/replace-conditional-with-polymorphism).
- **Principle:** SOLID - Open/Closed Principle.

## 📝 **Mini Task (Thought Exercise)**

Consider a shipping calculator based on a `shippingMethod` string ('UPS', 'FEDEX', 'USPS').

1. If you refactored this using Polymorphism, what would the **base class** be called?
2. What **method** would it define?
3. What would the **concrete classes** be called?
4. How would the **client code** change?
