# 💡 SOLID02.1: S - The Single Responsibility Principle

**Outline:**

- **The Smell (SEEI):** Identifying the "God Class," a class that does too much.
- **The Refactor (PPP):** Applying the Single Responsibility Principle (SRP) to create focused, maintainable classes.
- **Your Refactoring Mission (Production):** Time to break up a "God Class."

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Smell (SEEI)**

**The Code Smell: The God Class (or The Everything Class)**
This smell occurs when a class has taken on too many responsibilities. It knows too much and does too much. For example, a single `User` class might handle user profile data, but also know how to save itself to the database, send notification emails, and format its data for an admin report. It becomes the central, complex hub for too many unrelated concepts.

**The Negative Impact: Why is this a huge problem?**
God Classes are brittle and lead to cascading changes:

- **High Coupling:** Everything is connected to the God Class, so a change in one part of the system (like the database) forces a change in this massive class, which can break unrelated functionality (like email notifications).
- **Low Cohesion:** The methods and properties inside the class are not closely related. They don't have a single, unified purpose, making the class hard to understand and reason about.
- **Difficult to Test:** Unit testing a class that connects to a database AND an email server is a nightmare. You need complex mocks and setups.
- **Merge Conflicts:** In a team environment, everyone is constantly editing this one file, leading to frequent and painful merge conflicts.

**Example of the Smell: The `Employee` God Class**
This `Employee` class mixes three distinct responsibilities: employee data structure, database logic, and HR reporting logic.

```ts
class Employee {
  public name: string;
  public salary: number;

  constructor(name: string, salary: number) {
    this.name = name;
    this.salary = salary;
  }

  // Responsibility 1: Database Logic
  public saveToDatabase() {
    console.log(`Saving ${this.name} to the database...`);
    // ... complex database connection and INSERT/UPDATE logic
  }

  // Responsibility 2: HR Reporting Logic
  public generatePaySlip_HTML(): string {
    console.log(`Generating pay slip for ${this.name}...`);
    return `<div><h1>Pay Slip</h1><p>Name: ${this.name}</p><p>Salary: $${this.salary}</p></div>`;
  }
}
```

### **Part 2: Practice - The Refactor (PPP)**

**The Technique: The Single Responsibility Principle (SRP)**
The SRP states: **"A class should have only one reason to change."**

This doesn't mean a class should only have one method. It means all the methods and properties in a class should be related to a single _responsibility_ or _domain concept_. If you can think of more than one "reason" why you would need to modify a class, it's likely violating SRP.

For our `Employee` class, the reasons to change are:

1. The database schema changes.
2. The format of the HTML pay slip changes.
3. The core employee data fields change.
   That's three reasons, so we need to refactor.

**Step-by-Step Implementation: Breaking Down the `Employee` Class**
We will split the class into three new classes, each with a single, clear responsibility.

```ts
// 1. The Employee Data Class (a simple data structure)
// Its ONLY reason to change is if the definition of an employee changes.
class Employee {
  public name: string;
  public salary: number;

  constructor(name: string, salary: number) {
    this.name = name;
    this.salary = salary;
  }
}

// 2. The Employee Repository (handles database logic)
// Its ONLY reason to change is if the database interaction changes.
class EmployeeRepository {
  public save(employee: Employee) {
    console.log(`Saving ${employee.name} to the database...`);
    // ... database logic now lives here
  }
}

// 3. The Pay Slip Generator (handles HR reporting logic)
// Its ONLY reason to change is if the pay slip format changes.
class PaySlipGenerator {
  public generate_HTML(employee: Employee): string {
    console.log(`Generating pay slip for ${employee.name}...`);
    return `<div><h1>Pay Slip</h1><p>Name: ${employee.name}</p><p>Salary: $${employee.salary}</p></div>`;
  }
}
```

## 🧠 **Real-World Case Study: "Before vs. After Refactoring"**

- **Before (The God Class):**
  ```ts
  const emp = new Employee("Alice", 50000);
  emp.saveToDatabase();
  const paySlip = emp.generatePaySlip_HTML();
  ```
- **After (Focused Classes):**

  ```ts
  const emp = new Employee("Alice", 50000);

  const repository = new EmployeeRepository();
  repository.save(emp);

  const generator = new PaySlipGenerator();
  const paySlip = generator.generate_HTML(emp);
  ```

- **The Improvement:**
  - **Clarity and Focus:** Each class now does one thing and does it well. It's immediately obvious where to look if you need to change database logic.
  - **Testability:** We can easily test `PaySlipGenerator` with a mock `Employee` object without needing a database connection.
  - **Flexibility:** If we need to add a PDF pay slip generator, we just create a new `PaySlipPdfGenerator` class. We don't have to touch the `Employee` or `EmployeeRepository` classes at all.

## 🤔 **Reflective Questions**

1. The principle is "Single Responsibility," not "Single Method." What is the key difference? Can you think of a class with many methods that still follows SRP?
2. How does following SRP make your code easier for new developers on your team to understand?
3. SRP is often about separating "business logic" from "persistence logic" or "presentation logic." Why is this separation a powerful design pattern?

## 📚 **Further Reading**

- **The Bible:** "Clean Code" by Robert C. Martin has an excellent chapter dedicated to SRP and classes.
- **Code Quality Tool:** [NDepend for .NET](https://www.ndepend.com/) (and similar static analysis tools) can analyze class coupling and cohesion to help you spot potential SRP violations.
- **SOLID Principles:** A great overview article: [The S in SOLID](https://www.google.com/search?q=https://www.baeldung.com/solid-principles%231-single-responsibility-principle)

## 📝 **Mini Task (Production)**

You are given a `Report` class that is violating the Single Responsibility Principle. It's responsible for fetching data, parsing the data, and then printing the report to the console.

**Your Mission:**
Refactor this `Report` class into multiple smaller classes, where each new class adheres to the Single Responsibility Principle. Identify the core responsibilities and create a class for each one.

```ts
// BEFORE - Your starting point
class Report {
  public title: string;
  public data: string[];

  constructor(title: string) {
    this.title = title;
    this.data = [];
  }

  // Responsibility 1: Data Fetching
  fetchDataFromSource() {
    console.log("Fetching raw data...");
    // Simulates fetching some raw data
    this.data = ["line1,data1", "line2,data2"];
  }

  // Responsibility 2: Data Parsing
  parseData() {
    console.log("Parsing data...");
    this.data = this.data.map((line) => line.replace(",", ": "));
  }

  // Responsibility 3: Report Presentation
  printReport() {
    console.log(`--- ${this.title} ---`);
    this.data.forEach((line) => console.log(line));
    console.log(`--- End of Report ---`);
  }
}
```
