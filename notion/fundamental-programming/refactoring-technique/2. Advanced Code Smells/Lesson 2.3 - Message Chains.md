# 💡 ACS02.3: Message Chains

**Outline:**

- **The Smell (SEEI):** Identifying "Message Chains" (or "Train Wrecks") as a violation of the Law of Demeter.
- **The Refactor (PPP):** Applying the "Hide Delegate" technique to simplify client code and reduce coupling.
- **Your Refactoring Mission (Production):** Time to shorten a message chain.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Smell (SEEI)**

**The Code Smell: Message Chain (or "Train Wreck")**
A message chain occurs when client code navigates through a series of objects to get to the data it wants (e.g., `user.getProfile().getAddress().getCountryCode()`). This tightly couples the client code to the _entire navigation path_. If any object in the middle changes, the client code breaks.

**The Negative Impact: The Brittle Navigation Path**

- **High Coupling:** The client knows too much about the internal relationships of other classes.
- **Fragility:** If `Profile` no longer has an `Address` directly, every chain relying on that path breaks.
- **Verbose Code:** Long chains are harder to read and maintain.
- **Violation of Law of Demeter:** A method should only talk to its "immediate friends" and not "talk to strangers."

**Example of the Smell: Getting the Manager's Department**
To find an employee's manager's department, the client must navigate through three objects.

```ts
class Department {
  constructor(public name: string) {}
}
class Manager {
  constructor(public department: Department) {}
}
class Employee {
  constructor(public manager: Manager) {}
}

// Client Code
const employee = new Employee(new Manager(new Department("Engineering")));

// THE SMELL: A message chain
const departmentName = employee.manager.department.name;
```

### **Part 2: Practice - The Refactor (PPP)**

**The Technique: Hide Delegate**
Hide the navigation path from the client. The first object in the chain should provide a high-level method that handles the navigation internally.

1. **Identify the Chain:** Find the long `a.b.c.d` expression.
2. **Create a Method on the First Object:** Create a method that encapsulates the chain.
3. **Implement the Delegation:** Perform the navigation inside that method.
4. **Update the Client:** Call the simple method on the first object.

**Step-by-Step Implementation: Hiding the Department Navigation**
Add a method to the `Employee` class.

```ts
class Employee {
  constructor(public manager: Manager) {}

  // Step 2 & 3: Hide the delegation
  public getManagerDepartmentName(): string {
    return this.manager.department.name;
  }
}

// Client Code
const departmentName = employee.getManagerDepartmentName();
```

## 🧠 **Real-World Case Study: "Before vs. After Refactoring"**

- **Before (Message Chain):**
  ```ts
  const streetName = invoice.getCustomer().getAddress().getStreet();
  ```
- **After (Hide Delegate):**

  ```ts
  // In Invoice class:
  public getCustomerStreet(): string {
    return this.customer.getAddress().getStreet();
  }

  // Client code:
  const streetName = invoice.getCustomerStreet();
  ```

- **The Improvement:**
  - **Decoupling:** The client only knows `Invoice`; it doesn't care about `Customer` or `Address`.
  - **Simplicity:** The client code is cleaner.
  - **Centralized Logic:** Navigation logic lives in one place (DRY).

## 🤔 **Reflective Questions**

1. What is the Law of Demeter?
2. Can "Hide Delegate" be applied too aggressively?
3. How does a message chain indicate misplaced responsibilities?

## 📚 **Further Reading**

- **The Bible:** Fowler's "Refactoring" - "Hide Delegate" chapter.
- **Refactoring.guru:** [Message Chains examples](https://refactoring.guru/smells/message-chains).
- **Law of Demeter:** Also known as the Principle of Least Knowledge.

## 📝 **Mini Task (Production)**

Refactor the code below by applying "Hide Delegate" to remove the message chain.

**Your Mission:**

1. Add a method to `AppConfig` that provides the SMTP host directly.
2. Update the client code.

```ts
interface SmtpSettings {
  host: string;
  port: number;
}
interface NotificationSettings {
  smtp: SmtpSettings;
}

class AppConfig {
  public notifications: NotificationSettings;
  constructor() {
    this.notifications = {
      smtp: { host: "smtp.example.com", port: 587 },
    };
  }
}

// Client Code
const config = new AppConfig();
const smtpHost = config.notifications.smtp.host;
```
