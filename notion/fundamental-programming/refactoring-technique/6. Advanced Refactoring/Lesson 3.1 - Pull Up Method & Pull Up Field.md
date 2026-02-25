# 💡 AR03.1: Pull Up Method & Pull Up Field

**Outline:**

- **The Smell (SEEI):** Identifying duplicated methods or fields across multiple subclasses of the same superclass.
- **The Refactor (PPP):** Applying the "Pull Up Method" and "Pull Up Field" refactorings to centralize common code.
- **Your Refactoring Mission (Production):** Time to clean up a duplicated method in a class hierarchy.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Smell (SEEI)**

**The Code Smell: Duplication in Siblings**
This occurs when two or more subclasses share the exact same method or field. For example, if both `SalariedEmployee` and `HourlyEmployee` have identical `printPayStub()` methods, you have this smell. It's a clear violation of the DRY principle.

**The Negative Impact**

- **Maintenance Burden:** Bug fixes must be manually copied across every single subclass. One missed update leads to inconsistent behavior.
- **Code Bloat:** Redundant code makes the system harder to navigate and understand.
- **Incorrect Abstraction:** The superclass is "leaky"—it fails to capture commonalities, forcing children to redefine the same logic.

**Example of the Smell**
A hierarchy where `name` and `getProfile()` are repeated in every child class instead of being defined once in the `Employee` base class.

### **Part 2: Practice - The Refactor (PPP)**

**The Technique: Pull Up Method / Pull Up Field**
Moving common functionality from subclasses into their immediate superclass.

1. **Identify Duplication:** Find a method/field that is identical in brother/sister classes.
2. **Move to Super:** Copy it to the superclass.
3. **Adjust visibility:** Use `protected` if subclasses need direct access, or `public` for the general interface.
4. **Delete from Subclasses:** Remove the redundant copies.
5. **Verify:** Ensure behavior remains consistent across all types.

## 🧠 **Real-World Case Study: "The Clean Hierarchy"**

- **Before (Duplicated Logic):**
  Every `Employee` subclass has its own `name` field and `getProfile()` method. The code is bulky and repetitive.
- **After (Centralized in Superclass):**
  ```ts
  abstract class Employee {
    public name: string;
    constructor(name: string) {
      this.name = name;
    }

    public getProfile() {
      return `${this.getType()}: ${this.name}`;
    }
  }
  ```
- **The Improvement:** Duplication is dead. The shared concept of an employee is now correctly modeled. The subclasses are now small, focused, and only contain what makes them unique.

## 🤔 **Reflective Questions**

1. How does pulling up a method affect the "public interface" of the superclass?
2. What is the Template Method pattern, and how does it help when logic is _almost_ the same but not identical?
3. When should you NOT pull up a method even if it looks the same? (Hint: when the behavior's intent is actually different).

## 📚 **Further Reading**

- **The Bible:** Martin Fowler on Pull Up Method.
- **Refactoring.Guru:** [Pull Up Field](https://refactoring.guru/pull-up-field) and [Pull Up Method](https://refactoring.guru/pull-up-method).

## 📝 **Mini Task (Production)**

Refactor `Cat` and `Dog` sibling classes. They both have a duplicated `speak(message)` method.

```ts
class Dog extends Animal {
  public speak(msg) {
    console.log(`${this.name} says: ${msg}`);
  }
}
```

**Mission:**

1. Move the `speak` method to the `Animal` class.
2. Simplify the `Dog` and `Cat` subclasses.
3. Show the final `Animal` class with the shared method.
