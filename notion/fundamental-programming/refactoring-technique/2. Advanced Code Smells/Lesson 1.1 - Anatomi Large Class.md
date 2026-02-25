# 💡 ACS01.1: Anatomi Large Class

**Outline:**

- **The Smell (SEEI):** Defining the "Large Class" and how to measure it beyond just lines of code.
- **The "Refactor" (PPP):** Applying the analysis technique of "Responsibility Mapping."
- **Your Refactoring Mission (Production):** Time to perform an autopsy on a bloated class.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Smell (SEEI)**

**The Code Smell: Large Class**
A **Large Class** is a class that has grown so big that it's hard to understand, maintain, and test. While "lines of code" (LOC) is an easy first metric, a class's "size" is more accurately a measure of its **responsibilities**. A class with 500 lines of highly cohesive code might be fine, while a class with 200 lines juggling 5 different responsibilities is a problem. It's a prime violator of the Single Responsibility Principle.

**The Negative Impact: The Slowdown Effect**
Large classes become bottlenecks for development:

- **Low Cohesion:** Methods and properties are often unrelated, making it difficult to form a clear mental model.
- **High Coupling:** Because the class does so much, many other parts of the system depend on it, making it risky to change.
- **Difficult to Test:** You can't test one responsibility in isolation. A test for a reporting feature might require a full database and user session setup.
- **Constant Merge Conflicts:** The class becomes a "hotspot" where multiple developers make changes, leading to frequent and complex merges.

**Example of the Smell: The `OrderProcessor` Class**
This class started simple but has grown to handle data fetching, validation, payment processing, and notifications.

```ts
class OrderProcessor {
  private orderData: any;
  private customerData: any;

  // Responsibility 1: Data Loading
  public loadOrder(orderId: string) {
    /* Fetches from DB */
  }
  public loadCustomer(customerId: string) {
    /* Fetches from DB */
  }

  // Responsibility 2: Business Logic/Validation
  public validateOrderItems() {
    /* Checks stock, etc. */
  }
  public applyDiscounts() {
    /* Applies promo codes */
  }
  public calculateTotal() {
    /* ... */
  }

  // Responsibility 3: Payment Processing
  public chargeCreditCard(cardDetails: any) {
    /* Connects to payment gateway */
  }
  public processPaypalPayment(paypalToken: string) {
    /* ... */
  }

  // Responsibility 4: Notifications
  public sendOrderConfirmationEmail() {
    /* Sends email */
  }
  public sendShippingNotificationSms() {
    /* Sends SMS */
  }

  // ... and probably 20 more methods and 10 more properties ...
}
```

Its size isn't just about LOC; it's about the **four distinct jobs** it's performing.

### **Part 2: Practice - The "Refactor" (PPP)**

**The Technique: Responsibility Mapping**
Before you can refactor a Large Class, you must first dissect it. The "technique" is a structured analysis, not a code change. The goal is to identify cohesive clusters of methods and properties that represent a single responsibility.

1. **List all public methods.**
2. **List all private methods and instance variables (properties).**
3. **Group them:** For each public method, identify which other methods and properties it uses.
4. **Name the groups:** Look for clusters. (e.g., "Payment Processing", "Notifications").

**Step-by-Step Implementation: Mapping the `OrderProcessor`**

- **Group 1: Order & Customer Data Management**
  - `loadOrder()`, `loadCustomer()`
  - Properties: `orderData`, `customerData`
- **Group 2: Order Calculation & Validation**
  - `validateOrderItems()`, `applyDiscounts()`, `calculateTotal()`
- **Group 3: Payment Processing**
  - `chargeCreditCard()`, `processPaypalPayment()`
- **Group 4: Notifications**
  - `sendOrderConfirmationEmail()`, `sendShippingNotificationSms()`

This map is now our blueprint. It clearly shows that `OrderProcessor` should be broken into at least four smaller, more focused classes.

## 🧠 **Real-World Case Study: "Before vs. After Analysis"**

- **Before (The Monolithic View):**
  > "The OrderProcessor class is 800 lines long and has 30 methods. It's too big and hard to work with."
- **After (The Responsibility Map View):**
  > "The OrderProcessor class is violating SRP. It has four distinct responsibilities: Data Loading, Business Logic, Payment Processing, and Notifications. We can refactor this by creating new classes like `PaymentService` and `NotificationManager`."
- **The Improvement:** We have moved from a vague complaint to a concrete, actionable plan. We know _why_ it's too big and _how_ to start breaking it down.

## 🤔 **Reflective Questions**

1. Besides LOC and Number of Methods, what other metrics could indicate a class is too large?
2. Is it possible for a class to have many methods but still be highly cohesive? Provide an example.
3. How can team coding standards (e.g., "a class should not exceed 200 LOC") be both helpful and harmful?

## 📚 **Further Reading**

- **The Bible:** "Refactoring" by Martin Fowler, see the "Large Class" code smell.
- **Code Quality Tool:** [SonarQube](https://www.sonarqube.org/) analyses can flag Large Classes and calculate metrics like cyclomatic complexity.
- **SOLID Principles:** This smell is a direct violation of the **S** in SOLID (Single Responsibility Principle).

## 📝 **Mini Task (Production)**

Perform the "Responsibility Mapping" technique on the `ReportGenerator` class below.

**Your Mission:**

1. List all the public methods.
2. Group them into logical responsibilities.
3. Give each group a clear name.

```ts
class ReportGenerator {
  // Fetches data from different sources
  public connectToDatabase(connectionString: string): void {
    /* ... */
  }
  public fetchDataFromSql(query: string): any[] {
    /* ... */ return [];
  }
  public fetchDataFromApi(url: string): any[] {
    /* ... */ return [];
  }

  // Performs data transformation
  public parseSqlResults(data: any[]): any[] {
    /* ... */ return [];
  }
  public filterByDateRange(data: any[], start: Date, end: Date): any[] {
    /* ... */ return [];
  }
  public aggregateResults(data: any[]): any[] {
    /* ... */ return [];
  }

  // Formats the report into different outputs
  public formatAsHtml(data: any[]): string {
    /* ... */ return "";
  }
  public formatAsCsv(data: any[]): string {
    /* ... */ return "";
  }

  // Delivers the report
  public saveToFile(filename: string, content: string): void {
    /* ... */
  }
  public emailReport(recipient: string, content: string): void {
    /* ... */
  }
}
```
