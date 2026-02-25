# 💡 SR02.2: Pengantar Polimorfisme

**Outline:**

- **The Smell (SEEI):** Re-examining Conditional Hell as a failure to use polymorphism.
- **The "Refactor" (PPP):** Introducing Polymorphism as the fundamental OO principle for solving this problem.
- **Your Refactoring Mission (Production):** Time to implement a simple polymorphic structure.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Smell (SEEI)**

**The Code Smell: Asking for Type**
The root of "Conditional Hell" is code that **asks** an object for its type (`switch(obj.type)`) and makes decisions on its behalf. This is procedural thinking. In true OO design, you **tell** an object what to do and trust it to handle the details.

**The Negative Impact**

- **Centralizes Logic:** Creates brittle, central functions that change constantly.
- **Exposes Internals:** Forces objects to reveal a "type" property that should be private.
- **Breaks Encapsulation:** Data is in the object, but behavior is in a separate external `switch`.

**Example of the Smell (The "Asking" Style)**
A drawing function that checks types manually.

```ts
function drawShape(shape: Shape) {
  if (shape.type === "CIRCLE") {
    /* circle logic */
  } else if (shape.type === "SQUARE") {
    /* square logic */
  }
}
```

### **Part 2: Practice - The "Refactor" (PPP)**

**The Technique: Polymorphism**
Polymorphism ("many forms") allows us to present the same interface for different underlying data types.

1. **Define an Interface:** Declare a common method (e.g., `interface Shape { draw(): void }`).
2. **Implement Concrete Classes:** Each subclass provides its own `draw()` logic.
3. **Use the Interface:** Client code works with the interface, calling `shape.draw()` without caring about the concrete class.

**Step-by-Step Implementation**

1. Create `interface Shape { draw(): void }`.
2. Move circle logic to `Circle.draw()`.
3. Move square logic to `Square.draw()`.
4. Replace the `if/else` chain with `shape.draw()`.

## 🧠 **Real-World Case Study: "Before vs. After Refactoring"**

- **Before (Procedural "Asking"):**
  A `Greeter` class has a `switch` for languages (English, Spanish). To add French, you must modify the `Greeter`.
- **After (OO "Telling"):**

  ```ts
  interface Greeting {
    sayHello(): string;
  }
  class English implements Greeting {
    sayHello() {
      return "Hello";
    }
  }
  class Spanish implements Greeting {
    sayHello() {
      return "Hola";
    }
  }

  const greeting = myGreeting.sayHello();
  ```

- **The Improvement:** We've inverted the dependency. The high-level system depends on a simple abstraction (`Greeting`). Adding French only requires a new class, with no changes to existing code.

## 🤔 **Reflective Questions**

1. What's the difference between "telling" and "asking" an object?
2. How does polymorphism directly support the Open/Closed Principle?
3. Creating many classes is more "boilerplate." When is this trade-off worth it?

## 📚 **Further Reading**

- **GoF Patterns:** Polymorphism is the foundation of Design Patterns.
- **Refactoring.Guru:** [Replace Conditional with Polymorphism](https://refactoring.guru/replace-conditional-with-polymorphism).
- **YouTube:** [Visual Polymorphism Guide](https://www.youtube.com/watch?v=pTEtqz5S_44).

## 📝 **Mini Task (Production)**

Build a simple polymorphic notification system:

1. Interface `INotifier` with `send(msg: string): void`.
2. Classes `EmailNotifier`, `SmsNotifier`, and `PushNotifier` implementing it.
3. An array of `INotifier` containing one of each.
4. Loop through the array and call `.send("Test Message")`.

**Mission:** Observe how the same call (`.send()`) executes different logic based on the object's type.
