# 💡 SR03.3: Introduce Parameter Object

**Outline:**

- **The Smell (SEEI):** Identifying methods with "Long Parameter Lists," especially with related data.
- **The Refactor (PPP):** Applying the "Introduce Parameter Object" technique to group parameters into a cohesive class/interface.
- **Your Refactoring Mission (Production):** Time to bundle a group of related parameters into a single object.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Smell (SEEI)**

**The Code Smell: Long Parameter List**
This occurs when a method requires a long list of arguments (typically more than 3 or 4). This is often a sign the method is doing too much. A specific variant is when groups of parameters logically belong together (e.g., `x`, `y`, `radius`, `color`).

**The Negative Impact**

- **Hard to Read:** Signatures and calls become cluttered and difficult to scan.
- **Error-Prone:** It's easy to swap arguments of the same type (e.g., passing `height` into the `width` slot).
- **Inflexible:** Adding a new parameter requires updating every call site in the entire codebase.
- **Hides Concepts:** Using primitives (date1, date2) instead of a rich object (`DateRange`) hides the underlying business domain concept.

**Example of the Smell**
A filtering function with dates and flags spread across individual arguments.

```ts
function calculateEntries(
  entries: any[],
  start: Date,
  end: Date,
  includeAnomalies: boolean,
) {
  // ...
}
```

### **Part 2: Practice - The Refactor (PPP)**

**The Technique: Introduce Parameter Object**
Replace a group of related parameters with a single object (class or interface) that contains them.

1. **Identify** parameters that logically belong together (e.g., `startDate`, `endDate`).
2. **Create** an interface or class to represent the group (e.g., `interface DateRange`).
3. **Change Signature:** Update the method to accept the new object instead of individual primitives.
4. **Update Body:** Use the object's properties (e.g., `range.start`).
5. **Update Callers:** Change all call sites to pass the object.

**Step-by-Step Implementation**

1. Create `interface DateRange { start: Date; end: Date; }`.
2. Update `calculateEntries` signature.
3. Fix the method logic to use the `range` object.
4. Update the client code to pass a single object.

## 🧠 **Real-World Case Study: "Before vs. After Refactoring"**

- **Before (Long List):**
  A developer reading `calculateEntries(entries, d1, d2, true)` has to remember which date is start and which is end. Typo risk is high.
- **After (Parameter Object):**

  ```ts
  interface DateRange {
    start: Date;
    end: Date;
  }

  function calculateEntries(
    entries: any[],
    range: DateRange,
    includeAnomalies: boolean,
  ) {
    console.log(`Filtering from ${range.start} to ${range.end}`);
  }
  ```

- **The Improvement:** The signature is shorter and more expressive. It represents the concept of a "range" directly. This also gives you a perfect place to add helper methods like `.getDurationInDays()` later.

## 🤔 **Reflective Questions**

1. How does this refactoring make code "self-documenting"?
2. How does this relate to the "Primitive Obsession" smell?
3. When should you use a simple `interface` vs. a full `class` for the parameter object?

## 📚 **Further Reading**

- **The Bible:** Martin Fowler's "Refactoring".
- **Refactoring.Guru:** [Introduce Parameter Object](https://refactoring.guru/introduce-parameter-object).

## 📝 **Mini Task (Production)**

Refactor `createAccount` to group personal info into a `UserInfo` object.

```ts
function createAccount(
  fName: string,
  lName: string,
  email: string,
  sendEmail: boolean,
  planId: string,
) {
  // ...
}
```

**Mission:**

1. Create the `UserInfo` interface.
2. Group `fName`, `lName`, and `email`.
3. Update the function and shows the new, cleaner call site.
