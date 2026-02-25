# 💡 AR02.1: Long Parameter List Smell

**Outline:**

- **The Smell (SEEI):** Identifying and understanding the "Long Parameter List" code smell.
- **The "Refactor" (PPP):** This lesson focuses on diagnosis—recognizing which parameters naturally belong together.
- **Your Refactoring Mission (Production):** Time to analyze a method with too many inputs.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Smell (SEEI)**

**The Code Smell: Long Parameter List**
A very common and visible code smell where a method signature has too many parameters (typically > 3 or 4). This often indicates that the method is trying to do too much or that certain parameters represent a higher-level concept that hasn't been modeled as an object.

**The Negative Impact**

- **Poor Readability:** Hard to scan and understand what each value represents without checking the definition.
- **Error-Prone:** Easy to swap arguments of the same type (e.g., width vs height), causing subtle bugs.
- **High Maintenance:** Changes to signatures require updates across every single call site.
- **Hides Domain Concepts:** A list of primitives fails to capture rich business concepts like a `DateRange` or `Address`.

**Example of the Smell**
A booking function that requires everything from room IDs to customer names in a single, flat list.

```ts
function createBooking(
  room: string,
  start: Date,
  end: Date,
  fName: string,
  lName: string,
  email: string,
) {
  // ...
}
```

### **Part 2: Practice - The Refactor (PPP)**

**The Technique: Diagnosing Hidden Groups**
The first step is training your eyes to see "cliques" of data that belong together:

1. **Travel Together:** If `x` and `y` always appear as a pair across multiple methods, they belong in a `Point` object.
2. **Shared Prefix/Suffix:** `customerFirstName`, `customerLastName`, and `customerEmail` belong to a `CustomerInfo` object.
3. **Conceptual Units:** `checkInDate` and `checkOutDate` represent a single `BookingRange`.

Recognizing these groups is the crucial prerequisite for the **Introduce Parameter Object** refactoring.

## 🧠 **Real-World Case Study: "The Data Train Wreck"**

- **Scenario:** A function `drawRectangle` takes context, top/left coordinates, width/height, and various style flags.
- **The Grouping Opportunity:**
  - **Position:** `topLeftX`, `topLeftY` -> `Point`.
  - **Dimensions:** `width`, `height` -> `Size`.
  - **Style:** `fillColor`, `borderColor`, `borderWidth` -> `StyleOptions`.
- **The Lesson:** By identifying these three groups, you prepare the code to be refactored from 9 individual parameters down to 3 (or 4) meaningful objects. This makes the call site much safer and readable.

## 🤔 **Reflective Questions**

1. How does a long parameter list violate the Single Responsibility Principle?
2. How can TypeScript's type checking help mitigate (but not fully solve) this smell?
3. Is it ever acceptable to have a long list of parameters? (e.g., internal private methods).

## 📚 **Further Reading**

- **The Bible:** Martin Fowler on Long Parameter List.
- **Refactoring.Guru:** [Long Parameter List Smell](https://refactoring.guru/smells/long-parameter-list).
- **Blog:** [Primitive Obsession](https://refactoring.guru/smells/primitive-obsession) and why it leads to long lists.

## 📝 **Mini Task (Production)**

Analyze this function signature for a `Notification` service:

```ts
function sendEmail(
  to: string,
  from: string,
  subject: string,
  body: string,
  attachments: any[],
  isUrgent: boolean,
  retryCount: number,
): void;
```

**Mission:**

1. Identify at least two logical groups of parameters.
2. Suggest names for the interface/object that would represent each group.
3. Briefly explain why grouping them is better than leaving them as individual arguments.
