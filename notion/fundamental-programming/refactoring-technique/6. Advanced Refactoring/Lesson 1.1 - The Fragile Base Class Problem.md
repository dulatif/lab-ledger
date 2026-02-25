# 💡 AR01.1: The Fragile Base Class Problem

**Outline:**

- **The Smell (SEEI):** Understanding the "Fragile Base Class" problem, where changes in a superclass unexpectedly break its subclasses.
- **The "Refactor" (PPP):** This lesson focuses on diagnosis—recognizing the architectural flaw before changing code.
- **Your Refactoring Mission (Production):** Time to analyze a fragile hierarchy.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Smell (SEEI)**

**The Code Smell: The Fragile Base Class**
This is an insidious problem in OO design. It occurs when subclasses are tightly coupled not just to the public contract of their superclass, but to its internal implementation details. Inheritance creates one of the tightest forms of coupling in software.

**The Negative Impact**

- **High Coupling:** Subclass and superclass are fused. You cannot change one without risking the other.
- **Broken Encapsulation:** Superclass internals are leaked to children, preventing true data hiding.
- **Maintenance Nightmare:** Base classes become "frozen" because developers are afraid to modify them.
- **The Yo-Yo Problem:** To understand one method, you must jump up and down the inheritance chain.

**Example of the Smell**
A subclass `CountingList` that overrides `add` but relies on the base class `addAll` calling `add`.

```ts
class MyList {
  public add(item: any) {
    this.items.push(item);
  }
  public addAll(items: any[]) {
    items.forEach((i) => this.add(i));
  }
}

class CountingList extends MyList {
  private count = 0;
  public add(item: any) {
    this.count++;
    super.add(item);
  }
}
```

### **Part 2: Practice - The Refactor (PPP)**

**The Technique: Diagnosing the Flaw**
The fix is often to question the `extends` relationship itself. To diagnose, ask:

1. **How vs. What:** Does the subclass rely on _how_ the superclass works (implementation) or just _what_ it does (interface)?
2. **Internal Leakage:** Could a change in a `protected` method break the subclass?
3. **LSP Check:** Can I substitute a subclass instance anywhere a superclass instance is expected without error?

## 🧠 **Real-World Case Study: "The Optimized Break"**

- **Scenario:** A base class `MyList` has an `addAll` method that calls `add` in a loop.
- **Subclass:** `CountingList` overrides `add` to increment a counter. It works perfectly.
- **The Change:** A developer "optimizes" `MyList` by making `addAll` push all items directly into the array to avoid multiple function calls.
- **The Result:** `CountingList.add` is never called by `addAll`. The internal counter is now **wrong**, even though the public behavior of `MyList` hasn't changed.
- **The Lesson:** This hierarchy was "fragile" because the subclass depended on the private implementation detail of _how_ `addAll` was implemented.

## 🤔 **Reflective Questions**

1. What is the Liskov Substitution Principle (LSP)?
2. Why is inheritance coupling more dangerous than composition coupling?
3. Can `private` methods help prevent this problem?

## 📚 **Further Reading**

- **Effective Java:** "Favor composition over inheritance."
- **Wikipedia:** [Fragile Base Class](https://en.wikipedia.org/wiki/Fragile_base_class).
- **Principles:** [Liskov Substitution Principle](https://en.wikipedia.org/wiki/Liskov_substitution_principle).

## 📝 **Mini Task (Production)**

Analyze this hierarchy:

```ts
class Notification {
  protected addHeader(msg: string) {
    return `To: User\n${msg}`;
  }
  public send(msg: string) {
    console.log(this.addHeader(msg));
  }
}

class SmsNotification extends Notification {
  public sendFormatted(msg: string) {
    const h = this.addHeader(""); // Relies on addHeader format
    console.log(`${h}\n${msg.substring(0, 160)}`);
  }
}
```

**Mission:** Describe one "safe" change to the `Notification` base class that would break `SmsNotification`.
