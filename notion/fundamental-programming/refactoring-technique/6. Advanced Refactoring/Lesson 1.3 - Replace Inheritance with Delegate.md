# 💡 AR01.3: Replace Inheritance with Delegation

**Outline:**

- **The Smell (SEEI):** Identifying "Refused Bequest"—when a subclass inherits methods it doesn't want or need.
- **The Refactor (PPP):** A step-by-step guide to the "Replace Inheritance with Delegation" refactoring.
- **Your Refactoring Mission (Production):** Time to refactor a classic inheritance problem.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Smell (SEEI)**

**The Code Smell: Refused Bequest**
This occurs when a subclass inherits methods it doesn't use, or redefines them in a way that violates the superclass's contract. The subclass is "refusing its inheritance," often signaling that an "is-a" relationship is incorrect.

**The Negative Impact**

- **LSP Violation:** The subclass cannot be safely substituted for its parent because it doesn't share the same behavior.
- **Confusing API:** The subclass exposes methods that shouldn't be part of its public contract (e.g., a `Stack` inheriting `shift()` from an `Array`).
- **Leaky Abstraction:** Parent implementation details clutter the child's public API.

**Example of the Smell**
A `MyStack` inheriting from `Array` risks allowing clients to use `shift()`, which violates the LIFO (Last-In, First-Out) rule.

### **Part 2: Practice - The Refactor (PPP)**

**The Technique: Replace Inheritance with Delegation**
Turning a fragile "is-a" relationship into a flexible "has-a" compositional one.

1. **Create Member Field:** Add a private field for the delegate (e.g., `storage: Array`).
2. **Break Inheritance:** Remove the `extends` clause.
3. **Delegate Calls:** Update existing methods to use the member field instead of `super`.
4. **Forward Public API:** Explicitly create methods for any parent functionality you _actually_ want to expose.

## 🧠 **Real-World Case Study: "The Leaky Stack"**

- **Before (Inheritance):**
  `MyStack extends Array`. Clients can call `stack.shift()` or `stack.splice()`, which are not valid stack operations. The API is "fat" and dangerous.
- **After (Delegation):**

  ```ts
  class MyStack<T> {
    private storage: T[] = []; // Delegate

    public push(item: T) {
      this.storage.push(item);
    }
    public pop(): T | undefined {
      return this.storage.pop();
    }
    public peek(): T | undefined {
      return this.storage[this.storage.length - 1];
    }
  }
  ```

- **The Improvement:** The public API is now clean, intentional, and safe. Only `push`, `pop`, and `peek` are visible. Data integrity is guaranteed because the client can't reach the internal array.

## 🤔 **Reflective Questions**

1. How does this technique improve the "intentionality" of an API?
2. What is the main trade-off of this technique? (Hint: boilerplate).
3. Why is "has-a" often more truthful than "is-a" in complex business logic?

## 📚 **Further Reading**

- **Refactoring:** Martin Fowler on delegation.
- **Refactoring.Guru:** [Replace Inheritance with Delegation](https://refactoring.guru/replace-inheritance-with-delegation).
- **Principles:** Interface Segregation and Liskov Substitution Principle.

## 📝 **Mini Task (Production)**

Refactor this `PremiumUser` hierarchy. Currently, `PremiumUser extends User`.

1. Make `User` a standalone class.
2. Give `User` a `Membership` object component.
3. Show how to create a "GOLD" user using composition.

**Mission:** Show the final `User` class and how it combines with the `Membership` component to calculate the profile string.
