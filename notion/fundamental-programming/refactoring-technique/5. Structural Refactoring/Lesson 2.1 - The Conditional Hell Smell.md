# 💡 SR02.1: The Conditional Hell Smell

**Outline:**

- **The Smell (SEEI):** Identifying and understanding "Conditional Hell" and its violation of the Open/Closed Principle.
- **The "Refactor" (PPP):** Recognizing the smell's impact as the first step towards a solution.
- **Your Refactoring Mission (Production):** Time to analyze a problematic switch statement.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Smell (SEEI)**

**The Code Smell: Conditional Hell (Large Switch Statements or `if-else` Chains)**
This occurs when a function's primary purpose is to check an object's "type" or an attribute and execute different logic based on it. It indicates that one part of the system knows too much about the variations of another part.

**The Negative Impact**

- **Violates OCP (Open/Closed Principle):** You must modify existing code every time a new "type" is introduced.
- **Brittle and Fragile:** The function becomes a central point of failure. Changes for one `case` can break another.
- **High Maintenance:** Every new requirement becomes a bottleneck.
- **Violates SRP:** The function has multiple reasons to change (whenever any type's logic changes).
- **Duplication:** The same `switch` logic often spreads throughout the system, creating a maintenance nightmare.

**Example of the Smell**
A payment calculator that must be modified for every new user role.

```ts
function calculatePayment(user: User): number {
  switch (user.type) {
    case "ADMIN":
      return user.baseSalary * 1.5;
    case "EDITOR":
      return user.baseSalary * 1.1;
    case "GUEST":
      return 0;
  }
}
```

### **Part 2: Practice - The "Refactor" (PPP)**

**The Technique: Recognizing the Structural Problem**
Before fixing it, we must analyze the consequences. Ask these questions:

1. **New Type?** Do I have to find this `switch` to add it?
2. **Repeated?** Is this logic found in other files?
3. **Abstraction?** Does the `case` mix low-level logic with high-level flow?
4. **Testability?** Does every case need its own complex unit test?

Acknowledge that `switch` based on type codes is a structural design flaw. The next lessons will cover the solution: Polymorphism.

## 🧠 **Real-World Case Study: "The Spreading Sickness"**

- **Before (The Smell):**
  A Document Rendering system has a `renderComponent` switch. Later, a developer adds an `indexComponent` switch for search results.
- **The Problem:** Work is doubled. Adding a `VideoComponent` requires modifying TWO different functions/files. This design is not scalable.
- **The Improvement (The Goal):**
  A system where a `VideoComponent` class "knows" how to render and index itself. The main system doesn't need to change when a new component is added.

## 🤔 **Reflective Questions**

1. Why does a `switch` on type violate the Open/Closed Principle?
2. When is a `switch` actually acceptable? (e.g., non-type codes, static values).
3. How does this smell increase "onboarding time" for new developers?

## 📚 **Further Reading**

- **The Bible:** Martin Fowler's "Refactoring".
- **Design Principles:** [Open/Closed Principle](https://en.wikipedia.org/wiki/Open/closed_principle).
- **Blog:** [The Switch Statement is a Code Smell](https://lostechies.com/jimmybogard/2009/01/22/the-switch-statement-is-a-code-smell/).

## 📝 **Mini Task (Production)**

Analyze this `serializeShape` function. Document its weaknesses.

```ts
function serializeShape(shape: Shape): string {
  switch (shape.type) {
    case "CIRCLE":
      return `Radius: ${shape.radius}`;
    case "SQUARE":
      return `Side: ${shape.side}`;
  }
}
```

**Mission:**

1. List which SOLID principle is violated.
2. Steps needed to add a `Triangle`.
3. Risk of a developer adding the type but forgetting this function.
