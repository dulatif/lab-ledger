# 💡 AR01.2: Komposisi sebagai Alternatif

**Outline:**

- **The Smell (SEEI):** Revisiting the Fragile Base Class as a failure to choose a more flexible relationship.
- **The "Refactor" (PPP):** Introducing the design principle of "Favor Composition over Inheritance."
- **Your Refactoring Mission (Production):** Time to design a simple system using composition.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Smell (SEEI)**

**The Code Smell: The "Is-A" Straightjacket**
The root of the Fragile Base Class problem is locking into a rigid **"is-a"** relationship (e.g., `CountingList` _is a_ `MyList`). Inheritance is powerful but brittle—like wet concrete. Choosing this rigid structure when a more flexible one is available is the core mistake.

**The Negative Impact**

- **Inflexibility:** You cannot change an object's behavior at runtime.
- **Combinatorial Explosion:** Many dimensions of variation force a separate class for every possible combination (e.g., `EncryptedPdfReport`, `CsvReport`).
- **Tight Coupling:** Implementation details leak, causing fragility as seen in the previous lesson.

**Example of the Smell**
Locking a `Dog` to extend `Tail` and `Legs`. It's better to think about what an object **has** rather than what it **is**.

### **Part 2: Practice - The Refactor (PPP)**

**The Technique: Favor Composition over Inheritance**
Composition means building complex objects by combining simpler, independent parts. Instead of _being_ something else, an object _has_ other objects and delegates work to them.

1. **The Container (Context):** The outer object (e.g., `Car`).
2. **The Component (Collaborator):** The inner object (e.g., `Engine`).
3. **Delegation:** The container asks components to do the work (`car.start()` calls `engine.ignite()`).

The container only knows the component's **public interface**, keeping internals hidden and safe.

## 🧠 **Real-World Case Study: "Inheritance vs. Composition"**

- **Before (Fragile Inheritance):**
  `CountingList` extends `MyList`. When `MyList` optimizes its `addAll` method, it bypasses the overridden `add` method in `CountingList`, breaking the count.
- **After (Flexible Composition):**

  ```ts
  class CountingList {
    private items: any[] = [];
    private count = 0;

    public addAll(values: any[]) {
      this.items.push(...values); // Direct control
      this.count += values.length;
    }
  }
  ```

- **The Improvement:** `CountingList` is now in full control. It doesn't matter how the internal storage works; the logic is encapsulated inside the class. We can even swap out the internal `Array` for a `Set` without breaking clients.

## 🤔 **Reflective Questions**

1. How does composition improve encapsulation?
2. Why is limiting exposed methods (like NOT exposing `.pop()`) sometimes a good thing?
3. How do the **Strategy** and **Decorator** patterns use composition?

## 📚 **Further Reading**

- **GoF Patterns:** Composition is the master key to most patterns.
- **ThoughtWorks:** [Composition vs. Inheritance](https://www.thoughtworks.com/insights/blog/composition-vs-inheritance-how-choose).
- **YouTube:** [Composition Over Inheritance Guide](https://www.youtube.com/watch?v=wfMtDGfHWpA).

## 📝 **Mini Task (Production)**

Design a game `Character` using composition.

1. Create `IAttackBehavior` (Sword, Magic) and `IMoveBehavior` (Walk, Fly).
2. Create the main `Character` class that _has_ these behaviors.
3. Implement `performAttack()` and `performMove()` that delegate to the components.

**Mission:** Show the `Character` constructor and how it uses delegation to execute actions.
