# 💡 SR01.3: Menjaga Kohesi

**Outline:**

- **The Smell (SEEI):** Understanding "Low Cohesion" as the root cause for messy classes.
- **The "Refactor" (PPP):** Applying the principle of High Cohesion to guide your structural refactoring decisions.
- **Your Refactoring Mission (Production):** A planning exercise to identify responsibilities.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Smell (SEEI)**

**The Concept: Low Cohesion**
This isn't a single code smell but a fundamental design flaw that _causes_ "Large Class" and "Feature Envy." **Cohesion** measures how well the elements inside a single module (like a class) belong together.

- **High Cohesion (The Goal):** All methods and properties are closely related, working together for a single, well-defined purpose.
- **Low Cohesion (The Smell):** A class does many unrelated things (a "jack-of-all-trades"). A `Utils` class with date math, string parsing, and HTTP logic has extremely low cohesion.

**The Negative Impact**

- **Hard to Understand:** No clear mental model of what the class is for.
- **Hard to Maintain:** Changes in one responsibility require understanding other unrelated ones to avoid breakage.
- **Hard to Reuse:** To use one function, you must import the entire bloated class and its dependencies.
- **Ripple Effect:** Unseparated responsibilities cause changes to cascade and break unrelated parts of the system.

**Example of the Smell**
A `Report` class that holds data, formats HTML, AND saves to a database.

```ts
class Report {
  public toHtml() {
    /* Responsibility 1: HTML Formatting */
  }
  public saveToDatabase() {
    /* Responsibility 2: DB Persistence */
  }
  public getTitle() {
    /* Responsibility 3: Data Holder */
  }
}
```

### **Part 2: Practice - The Refactor (PPP)**

**The Guiding Principle: Maximize Cohesion**
Cohesion is a way of thinking that guides refactorings like **Extract Class** and **Move Method**.

1. **Look at the Data:** Which instance variables does a method use?
2. **Find Clustered Cliques:** Identify groups of methods that use one subset of variables and other groups that use another. These are your candidates for extraction.
3. **Analyze Vocabulary:** Methods called `format`, `render`, or `toHtml` belong in a "Presenter" or "Formatter." Methods called `save` or `load` belong in a "Repository."

## 🧠 **Real-World Case Study: "Before vs. After Refactoring"**

- **Before (Low Cohesion):**
  A single class doing three distinct jobs: data storage, presentation, and persistence.
- **After (High Cohesion):**

  ```ts
  class Report {
    /* Data only */
  }

  class ReportFormatter {
    public toHtml(report: Report) {
      /* Logic only */
    }
  }

  class ReportRepository {
    public save(report: Report) {
      /* Database only */
    }
  }
  ```

- **The Improvement:** Three classes, each with a single, focused responsibility. They are easier to test, maintain, and reuse independently.

## 🤔 **Reflective Questions**

1. What is the relationship between Cohesion and the Single Responsibility Principle (SRP)?
2. Does increasing cohesion tend to increase or decrease coupling?
3. Can a class have _too high_ cohesion? What does that look like?

## 📚 **Further Reading**

- **Clean Architecture:** Robert C. Martin on SRP and cohesion.
- **Wikipedia:** [Cohesion](<https://en.wikipedia.org/wiki/Cohesion_(computer_science)>).
- **Blog:** [Understanding Cohesion and Coupling](https://www.freecodecamp.org/news/cohesion-and-coupling-in-software-engineering/).

## 📝 **Mini Task (Production)**

Analyze the following `User` class. Identify the distinct responsibilities and group them into logical clusters (e.g., "Auth", "Profile", "Messaging").

```ts
class User {
  private userId: string;
  private hashedPassword_DONT_TOUCH: string; // auth
  private lastLoginIp: string; // auth
  private profileBio: string; // profile
  private smtpHost: string; // email config

  public checkPassword(p: string) {
    /* ... */
  }
  public updateProfile(bio: string) {
    /* ... */
  }
  public sendResetEmail() {
    /* ... */
  }
}
```

**Mission:** List the clusters and which fields/methods belong to each. This is the first step of structural refactoring.
