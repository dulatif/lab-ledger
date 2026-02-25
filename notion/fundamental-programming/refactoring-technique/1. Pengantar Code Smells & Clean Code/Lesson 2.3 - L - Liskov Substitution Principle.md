# 💡 SOLID02.3: L - The Liskov Substitution Principle

**Outline:**

- **The Smell (SEEI):** Identifying the "Deceptive Subclass" that breaks expectations.
- **The Refactor (PPP):** Applying the Liskov Substitution Principle (LSP) to create reliable inheritance hierarchies.
- **Your Refactoring Mission (Production):** Time to fix a broken substitution.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Smell (SEEI)**

**The Code Smell: The Deceptive Subclass**
This is a subtle but dangerous smell that occurs in inheritance. It happens when a subclass or subtype _behaves_ in a way that the client code does not expect. The client code is written to work with the parent class (the base type), but when it's given an instance of the subclass (the subtype), it breaks. The subclass violates the "contract" of its parent.

**The Negative Impact: Why is this so dangerous?**
Violating LSP undermines the entire point of polymorphism and inheritance:

- **Unexpected Bugs:** The code looks correct, but it fails at runtime with strange errors. This is extremely hard to debug.
- **Breaks Polymorphism:** It forces you to write code that checks the type of an object before using it (`if (obj instanceof Square)`), which defeats the purpose of using a base class.
- **Erodes Trust:** Developers lose trust in the type system. They can no longer assume that a `Rectangle` will always behave like a `Rectangle`.

**Example of the Smell: The Classic `Rectangle` and `Square` Problem**
A `Square` _is a_ `Rectangle` in a geometric sense, but not necessarily in a behavioral, object-oriented sense when setters are involved.

```ts
class Rectangle {
  protected width: number;
  protected height: number;

  constructor(width: number, height: number) {
    this.width = width;
    this.height = height;
  }

  public setWidth(width: number): void {
    this.width = width;
  }

  public setHeight(height: number): void {
    this.height = height;
  }

  public getArea(): number {
    return this.width * this.height;
  }
}

class Square extends Rectangle {
  // A square must have equal width and height, so we override the setters.
  public setWidth(width: number): void {
    this.width = width;
    this.height = width; // This breaks the parent's behavior!
  }

  public setHeight(height: number): void {
    this.width = height;
    this.height = height; // This also breaks the parent's behavior!
  }
}

// Client code that expects any Rectangle to behave normally
function useRectangle(rect: Rectangle) {
  rect.setWidth(5);
  rect.setHeight(4);
  // The client reasonably EXPECTS the area to be 5 * 4 = 20.
  const area = rect.getArea();
  console.log(`Expected area to be 20, but got ${area}`);
}
```

### **Part 2: Practice - The Refactor (PPP)**

**The Technique: The Liskov Substitution Principle (LSP)**
LSP states: **"Subtypes must be substitutable for their base types without altering the correctness of the program."** This means a subclass must adhere to the contract of its superclass.

A subclass shouldn't:

- Throw new exceptions that the superclass doesn't.
- Have stricter preconditions (require more from its inputs).
- Have weaker postconditions (guarantee less in its output).
- Change the fundamental behavior of inherited methods.

**Step-by-Step Implementation: Fixing the Shape Hierarchy**
The solution is often to rethink the inheritance. We can create a more abstract base class or interface.

```ts
// Solution: Create a more abstract, shared interface for all shapes.
interface Shape {
  getArea(): number;
}

// Now Rectangle and Square are independent peers implementing the same interface.
class Rectangle implements Shape {
  private readonly width: number;
  private readonly height: number;

  constructor(width: number, height: number) {
    this.width = width;
    this.height = height;
  }

  public getArea(): number {
    return this.width * this.height;
  }
}

class Square implements Shape {
  private readonly side: number;

  constructor(side: number) {
    this.side = side;
  }

  public getArea(): number {
    return this.side * this.side;
  }
}

// Client code now works with the abstraction "Shape"
function printArea(shape: Shape) {
  console.log(`The area is ${shape.getArea()}`);
}
```

## 🧠 **Real-World Case Study: "Before vs. After Refactoring"**

- **Before (LSP Violation):**
  ```ts
  function useRectangle(rect: Rectangle) {
    rect.setWidth(5);
    rect.setHeight(4);
    // This code is unreliable. It gives different results for Rectangle and Square.
    console.log(rect.getArea());
  }
  ```
- **After (LSP Compliant):**
  ```ts
  // No more problematic setters. The client works with a stable interface.
  function printArea(shape: Shape) {
    console.log(`The area is ${shape.getArea()}`);
  }
  ```
- **The Improvement:**
  - **Reliability:** The code now behaves predictably.
  - **Correct Abstraction:** The new design focuses on a shared capability (`getArea`) rather than a flawed inheritance structure.
  - **Simplicity:** The client code doesn't need to worry about the specific type of shape.

## 🤔 **Reflective Questions**

1. Inheritance is often described with an "is a" relationship. How does the `Square`/`Rectangle` problem show that "is a" can be misleading?
2. Bird/Penguin example: Why would a `Penguin` subclass with a `fly()` method potentially violate LSP?
3. How does violating LSP often lead to violations of the Open/Closed Principle?

## 📚 **Further Reading**

- **The Bible:** Robert C. Martin provides a deep dive in his books and articles.
- **Design Patterns:** LSP is fundamental to patterns like Strategy or Decorator.
- **SOLID Principles:** A great overview article: [The L in SOLID](https://www.google.com/search?q=https://www.baeldung.com/solid-principles%233-liskov-substitution-principle)

## 📝 **Mini Task (Production)**

You are given a class hierarchy for settings. `ReadOnlySettings` violates LSP because its `setSetting` throws an error.

**Your Mission:**
Refactor the hierarchy so that code expecting write access doesn't get a read-only object by mistake. Hint: Create separate interfaces for reading and writing.

```ts
class Settings {
  protected settings: Map<string, string> = new Map();

  public getSetting(key: string): string | undefined {
    return this.settings.get(key);
  }

  public setSetting(key: string, value: string): void {
    console.log(`Setting ${key} to ${value}`);
    this.settings.set(key, value);
  }
}

class ReadOnlySettings extends Settings {
  public setSetting(key: string, value: string): void {
    // This is the violation!
    throw new Error("Cannot modify read-only settings!");
  }
}
```
