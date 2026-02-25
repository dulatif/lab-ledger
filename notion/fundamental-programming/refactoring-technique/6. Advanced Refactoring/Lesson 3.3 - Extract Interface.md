# 💡 AR03.3: Extract Interface

**Outline:**

- **The Smell (SEEI):** A client is coupled to a specific, concrete class when it only needs a subset of its behavior.
- **The Refactor (PPP):** Applying the "Extract Interface" refactoring to decouple the client from the implementation.
- **Your Refactoring Mission (Production):** Time to extract a common contract from a concrete class.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Smell (SEEI)**

**The Code Smell: Depending on a Concrete Class**
This occurs when high-level code depends directly on a specific lower-level implementation rather than an abstraction. This is a violation of the **Dependency Inversion Principle**. For example, a `NotificationService` that is hard-coded to work only with `EmailSender`.

**The Negative Impact**

- **Inflexibility:** Supporting a new notification type (like SMS) requires modifying the service itself.
- **Poor Testability:** Testing the service forces you to use the real "heavy" dependencies (like the actual email server), making tests slow and brittle.
- **Limited Reusability:** The service is trapped in a specific environment (e.g., "only for email").

**Example of the Smell**
A class that instantiates its own dependencies internally using the `new` keyword, making it impossible to swap them out for testing or new features.

### **Part 2: Practice - The Refactor (PPP)**

**The Technique: Extract Interface**
Introducing a layer of abstraction between the client and the service.

1. **Define Interface:** Create a new `interface` declaring only the methods the client actually uses.
2. **Implement:** Mark the concrete class as implementing that interface.
3. **Change Type:** Update the client's field and parameter types to use the interface.
4. **Inject Dependency:** Pass the concrete implementation into the client's constructor (Dependency Injection).

## 🧠 **Real-World Case Study: "Email vs. SMS"**

- **Before (Tight Coupling):**
  `NotificationService` has a private `EmailSender` field. It cannot send anything else.
- **After (Loose Coupling):**

  ```ts
  interface IMessageSender {
    sendMessage(to: string, msg: string): void;
  }

  class NotificationService {
    constructor(private sender: IMessageSender) {} // Injected!

    public notify(user: string, msg: string) {
      this.sender.sendMessage(user, msg);
    }
  }
  ```

- **The Improvement:** The service now works with _any_ sender. We can add `SmsSender` or `SlackSender` without changing a single line of `NotificationService`. Testing is simple: we just inject a "mock" sender.

## 🤔 **Reflective Questions**

1. How does this refactoring enable the **Dependency Inversion Principle**?
2. What is Dependency Injection, and why is it the natural partner of "Extract Interface"?
3. When is an `abstract class` better than an `interface` for abstraction?

## 📚 **Further Reading**

- **Refactoring:** Martin Fowler on Extract Interface.
- **Refactoring.Guru:** [Extract Interface](https://refactoring.guru/extract-interface).
- **SOLID:** The Dependency Inversion Principle.

## 📝 **Mini Task (Production)**

Decouple `DataProcessor` from `FileLogger`.

```ts
class DataProcessor {
  private logger = new FileLogger();
  // ...
}
```

**Mission:**

1. Create `ILogger` interface.
2. Make `FileLogger` implement it.
3. Update `DataProcessor` to take `ILogger` in its constructor.
4. Show how to initialize the processor with the logger.
