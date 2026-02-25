# 💡 AR02.2: Introduce Parameter Object

**Outline:**

- **The Smell (SEEI):** A quick review of "Long Parameter List" where data clumps are passed as individual parameters.
- **The Refactor (PPP):** A step-by-step guide to the "Introduce Parameter Object" refactoring.
- **Your Refactoring Mission (Production):** Time to bundle a messy parameter list into a clean object.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Smell (SEEI)**

**The Code Smell: Data Clumps**
This is the specific smell we diagnosed previously. A "data clump" is any group of variables (like `firstName`, `lastName`, `email`) that are frequently passed around together. When you see a clump, it's a sign that you're missing a class or interface in your domain model.

**The Negative Impact**

- **Reduces Readability:** Method calls like `createBooking(customerInfo, dateRange)` are much cleaner than lists of 7+ primitives.
- **Increases Boilerplate:** Every time you pass the clump to another function, you have to relay every single variable again.
- **No Centralized Logic:** Primitives have no "home" for methods. If you have a `CustomerInfo` object, you have a logical place for a `getFullName()` method.

**Example of the Smell**
A `createBooking` function receiving individual strings and dates that clearly belong together.

### **Part 2: Practice - The Refactor (PPP)**

**The Technique: Introduce Parameter Object**
Replacing a group of parameters with a single object that encapsulates them.

1. **Define Object:** Create an `interface` or `class` (e.g., `CustomerInfo`).
2. **Add Fields:** Include all the group's primitives as properties.
3. **Change Signature:** Update the method to take the new object.
4. **Update Body:** Access data via the object (e.g., `customer.firstName`).
5. **Update Callers:** Create instances of the object at every call site.

## 🧠 **Real-World Case Study: "Before vs. After Refactoring"**

- **Before (Data Clumps):**
  `createBooking("101", checkIn, checkOut, "John", "Doe", email, true)`. The developer has to guess which argument maps to which parameter.
- **After (Parameter Objects):**

  ```ts
  interface BookingRange {
    start: Date;
    end: Date;
  }
  interface CustomerInfo {
    name: string;
    email: string;
  }

  function createBooking(
    id: string,
    range: BookingRange,
    customer: CustomerInfo,
  ) {
    console.log(`Booking for ${customer.name}`);
  }
  ```

- **The Improvement:** The signature is now expressive and shorter. We've created reusable domain concepts (`BookingRange`, `CustomerInfo`) that can be used across the entire system.

## 🤔 **Reflective Questions**

1. When should you choose a `class` over a simple `interface` for a parameter object?
2. How does this refactoring help you discover new classes in your domain?
3. How can this simplify unit testing?

## 📚 **Further Reading**

- **The Bible:** Martin Fowler on Introduce Parameter Object.
- **Refactoring.Guru:** [Introduce Parameter Object](https://refactoring.guru/introduce-parameter-object).

## 📝 **Mini Task (Production)**

Refactor the `drawRectangle` function from the previous lesson.

```ts
function drawRectangle(ctx, x, y, width, height, fill, border, bWidth, shadow) {
  // ...
}
```

**Mission:**

1. Create interfaces for `Point`, `Dimensions`, and `StyleOptions`.
2. Rewrite the function signature to use these objects.
3. Show the new, readable call site.
