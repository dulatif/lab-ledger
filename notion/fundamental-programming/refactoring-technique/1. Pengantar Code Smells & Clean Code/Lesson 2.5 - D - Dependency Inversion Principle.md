# 💡 SOLID02.5: D - The Dependency Inversion Principle

**Outline:**

- **The Smell (SEEI):** Identifying the "Concrete Jungle," where high-level logic is tightly coupled to low-level details.
- **The Refactor (PPP):** Applying the Dependency Inversion Principle (DIP) to decouple modules via abstractions.
- **Your Refactoring Mission (Production):** Time to invert some dependencies.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Smell (SEEI)**

**The Code Smell: The Concrete Jungle**
This smell exists when your high-level business logic modules depend directly on low-level implementation details. For example, a high-level `ReportGenerator` class directly creates an instance of a low-level `MySqlDatabase` class within its own code (`const db = new MySqlDatabase()`).

**The Negative Impact: Why is this tight coupling so bad?**
It makes your code inflexible, fragile, and extremely difficult to test:

- **Inflexibility:** If the `ReportGenerator` is hard-coded to use `MySqlDatabase`, switching to `PostgreSqlDatabase` requires changing the high-level code.
- **Fragility:** Changes in the low-level module can break your business logic.
- **Untestability:** You cannot unit test the `ReportGenerator` without a real database. The business logic is permanently tied to an external dependency.

**Example of the Smell: The `NotificationService`**
This high-level service is tightly coupled to the low-level `EmailClient`.

```ts
// Low-level detail
class EmailClient {
  public sendEmail(to: string, body: string) {
    console.log(`Sending email to ${to}: ${body}`);
  }
}

// High-level business logic
class NotificationService {
  // Direct dependency on a concrete class.
  private emailClient: EmailClient = new EmailClient();

  public sendWelcomeNotification(userEmail: string) {
    this.emailClient.sendEmail(userEmail, "Welcome to our platform!");
  }
}
```

### **Part 2: Practice - The Refactor (PPP)**

**The Technique: The Dependency Inversion Principle (DIP)**
The DIP has two parts:

1. **High-level modules should not depend on low-level modules. Both should depend on abstractions (interfaces).**
2. **Abstractions should not depend on details. Details should depend on abstractions.**

Instead of `A --> B`, we want `A --> I <-- B`. The control of which implementation to use is handled outside the class, a concept called **Inversion of Control (IoC)**, often achieved with **Dependency Injection (DI)**.

**Step-by-Step Implementation: Inverting the Dependency**

1. **Define an Abstraction:** Create an interface (e.g., `IMessageSender`).
2. **Depend on the Abstraction:** The `NotificationService` depends on the interface and receives an implementation via its constructor.
3. **Implement the Abstraction:** The `EmailClient` implements the interface.

```ts
// Step 1: Define an abstraction
interface IMessageSender {
  sendMessage(recipient: string, message: string): void;
}

// Step 3: The low-level detail now depends on the abstraction
class EmailClient implements IMessageSender {
  public sendMessage(recipient: string, message: string) {
    console.log(`Sending email to ${recipient}: ${message}`);
  }
}

// Step 2: The high-level module depends on the abstraction
class NotificationService {
  private messageSender: IMessageSender;

  // The dependency is "injected" via the constructor.
  constructor(sender: IMessageSender) {
    this.messageSender = sender;
  }

  public sendWelcomeNotification(userEmail: string) {
    this.messageSender.sendMessage(userEmail, "Welcome to our platform!");
  }
}
```

## 🧠 **Real-World Case Study: "Before vs. After Refactoring"**

- **Before (Tightly Coupled):**
  ```ts
  const notificationService = new NotificationService();
  notificationService.sendWelcomeNotification("test@example.com");
  ```
- **After (Decoupled with DI):**

  ```ts
  const emailSender = new EmailClient();
  const notificationService = new NotificationService(emailSender);
  notificationService.sendWelcomeNotification("test@example.com");

  // Switch to SMS without changing NotificationService!
  class SmsClient implements IMessageSender {
    public sendMessage(recipient: string, message: string) {
      console.log(`Sending SMS to ${recipient}: ${message}`);
    }
  }
  const smsNotificationService = new NotificationService(new SmsClient());
  ```

- **The Improvement:**
  - **Testability:** Easily test `NotificationService` by injecting a mock sender.
  - **Reusability:** The service is no longer tied to email.
  - **Pluggable Architecture:** System components are like Lego blocks.

## 🤔 **Reflective Questions**

1. What is the difference between DIP, IoC, and DI?
2. What is the purpose of "DI Containers" in frameworks like NestJS?
3. How does following the Dependency Inversion Principle help you follow the Open/Closed Principle?

## 📚 **Further Reading**

- **The Bible:** Robert C. Martin's "The Dependency Inversion Principle" (PDF).
- **Design Patterns:** Fowler's article on "Inversion of Control Containers and the Dependency Injection pattern."
- **SOLID Principles:** A great overview article: [The D in SOLID](https://www.google.com/search?q=https://www.baeldung.com/solid-principles%235-dependency-inversion-principle)

## 📝 **Mini Task (Production)**

You are given a high-level `DataProcessor` tightly coupled to a `FileLogger`.

**Your Mission:**
Decouple them by introducing an `ILogger` interface and injecting it into the `DataProcessor` constructor.

```ts
class FileLogger {
  public log(message: string): void {
    console.log(`(File Log): ${message}`);
  }
}

class DataProcessor {
  private logger: FileLogger = new FileLogger();

  public process(data: string) {
    this.logger.log(`Starting processing for data: ${data}`);
    // ... logic ...
    this.logger.log("Processing complete.");
  }
}
```
