# 💡 ACS01.4: Long Parameter List & Data Clumps

**Outline:**

- **The Smell (SEEI):** Identifying "Long Parameter Lists" and "Data Clumps" as symptoms of missing objects.
- **The Refactor (PPP):** Applying "Introduce Parameter Object" to group related data.
- **Your Refactoring Mission (Production):** Time to bundle up some parameters.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Smell (SEEI)**

**The Code Smell: Long Parameter List & Data Clumps**
These two smells are closely related:

- **Long Parameter List:** A function has too many parameters (typically > 3 or 4), making it hard to read and easy to get wrong (e.g., swapping parameters of the same type).
- **Data Clumps:** Groups of variables that always travel together (e.g., `startDate, endDate` or `x, y, width, height`). This indicates a missing concept in your domain model.

**The Negative Impact: Noise and Missed Abstractions**

- **Readability:** `createEvent(..., ..., ..., ...)` is noisy. `createEvent(eventDetails)` is clean.
- **Brittleness:** Adding a new piece of data to a clump (e.g., a `z` coordinate) requires changing every function that uses that clump.
- **Hidden Relationships:** The implicit relationship between clumped items (like `startX` and `startY` forming a `Point`) is not enforced without an object.

**Example of the Smell: The `drawRectangle` function**
This function has a long parameter list with two data clumps for points.

```ts
function drawRectangle(
  // First data clump
  startX: number,
  startY: number,
  // Second data clump
  endX: number,
  endY: number,
  // Other parameters
  color: string,
  borderWidth: number,
) {
  /* logic */
}
```

The parameters `startX` and `startY` always travel together and should be an object.

### **Part 2: Practice - The Refactor (PPP)**

**The Technique: Introduce Parameter Object**
When you spot a data clump, create a class or interface to represent that concept.

1. **Identify the Data Clump.**
2. **Create a New Class/Interface:** Give it a meaningful name (e.g., `Point`, `DateRange`).
3. **Modify the Function Signature:** Accept the new parameter object instead of individual parameters.
4. **Update Call Sites.**

**Step-by-Step Implementation: Refactoring `drawRectangle`**
Bundle coordinates into a `Point` object.

```ts
// Step 2: Create a new interface for the concept of a "Point"
interface Point {
  x: number;
  y: number;
}

// Step 3: Modify the function to use the new parameter object
function drawRectangle(
  startPoint: Point,
  endPoint: Point,
  color: string,
  borderWidth: number,
) {
  // e.g., context.moveTo(startPoint.x, startPoint.y);
}

// Step 4: Update the calling code
drawRectangle({ x: 10, y: 10 }, { x: 100, y: 50 }, "blue", 2);
```

## 🧠 **Real-World Case Study: "Before vs. After Refactoring"**

- **Before (Long Parameter List):**
  ```ts
  function createBooking(
    customerId: string,
    roomId: string,
    checkInDate: Date,
    checkOutDate: Date,
    discountCode?: string,
  ) {
    /* ... */
  }
  ```
- **After (Parameter Object):**
  ```ts
  interface BookingRequest {
    customerId: string;
    roomId: string;
    dateRange: { start: Date; end: Date };
    discountCode?: string;
  }
  function createBooking(request: BookingRequest) {
    /* ... */
  }
  ```
- **The Improvement:**
  - **Readability:** The signature is short and expresses a clear concept: `BookingRequest`.
  - **Flexibility:** Adding a field (like `numberOfGuests`) only changes the interface, not the function signature.
  - **Domain Modeling:** You've identified a valuable concept (`BookingRequest`) that can be reused.

## 🤔 **Reflective Questions**

1. What's the difference between "Introduce Parameter Object" and "Preserve Whole Object"?
2. When might a function with 4-5 parameters still be acceptable?
3. How do TypeScript interfaces make this refactoring lightweight?

## 📚 **Further Reading**

- **The Bible:** Fowler's "Refactoring" recipes for "Introduce Parameter Object."
- **Domain-Driven Design (DDD):** Identifying "Value Objects" from data clumps.
- **API Design:** Maintaining clean function signatures as a small API.

## 📝 **Mini Task (Production)**

Refactor the `createUser` function below using "Introduce Parameter Object".

**Your Mission:**

1. Identify the data clump.
2. Create an `Address` interface.
3. Create a `UserCreationParams` interface.
4. Rewrite the `createUser` signature.

```ts
function createUser(
  firstName: string,
  lastName: string,
  email: string,
  dateOfBirth: Date,
  // This is a data clump
  street: string,
  city: string,
  postalCode: string,
  country: string,
) {
  console.log(`Creating user ${firstName} living at ${street}, ${city}...`);
}
```
