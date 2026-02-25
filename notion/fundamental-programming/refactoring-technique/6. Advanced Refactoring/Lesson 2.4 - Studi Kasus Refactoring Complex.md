# 💡 AR02.4: Studi Kasus Refactoring Complex

**Outline:**

- **The Smell (SEEI):** A messy, procedural function call with a long parameter list, often for configuring a complex object or API request.
- **The Refactor (PPP):** Combining "Introduce Parameter Object" and the "Builder Pattern" to create a clean, readable, and fluent API.
- **Your Refactoring Mission (Production):** Time to wrap a complex function call in a simple, fluent builder.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Smell (SEEI)**

**The Code Smell: The Messy Function Call**
This is an advanced version of the "Long Parameter List." It occurs when configuring complex operations like HTTP requests or database queries. These operations often have many optional parameters, leading to "telescoping" constructors or unreadable function calls filled with `null` values.

**The Negative Impact**

- **Unreadable Code:** `fetchData(u, "GET", null, null, 5000, 3, false)` is impossible to decipher without checking the definition.
- **Error-Prone:** Misordering optional booleans or numbers is a common source of bugs.
- **Complex Setup:** The client code is forced to perform heavy procedural setup just to make one call.

**Example of the Smell**
A generic `fetchData` function where simple requests require passing 7+ arguments, half of which are often dummy `null` values.

### **Part 2: Practice - The Refactor (PPP)**

**The Technique: Parameter Object + Builder Pattern**
Construct complex objects step-by-step using a fluent interface.

1. **Group Parameters:** Create a `FetchOptions` class/interface to hold the data.
2. **Create Builder:** Create a `FetchRequestBuilder` class.
3. **Fluent Chaining:** Add methods that set properties and `return this` (e.g., `.withTimeout(5000)`).
4. **Final Build:** Add a `.build()` method that returns the completed options object.
5. **Simplify Receiver:** Update the original function to take only the one options object.

## 🧠 **Real-World Case Study: "The Fluent API"**

- **Before (Procedural Mess):**
  `fetchData("/api/posts", "GET", null, null, 5000, 3, false)`. What do those numbers and nulls even do?
- **After (Fluent Builder):**

  ```ts
  const request = new FetchRequestBuilder("/api/posts")
    .withTimeout(5000)
    .withoutCache()
    .build();

  fetchData(request);
  ```

- **The Improvement:** The code is now self-documenting. A developer can read the builder calls like a sentence. It hides implementation details and makes the API much harder to misuse.

## 🤔 **Reflective Questions**

1. How does the Builder pattern help with creating immutable objects?
2. What is a "fluent interface"?
3. When is the added complexity of a Builder class worth it?

## 📚 **Further Reading**

- **GoF:** The Builder Pattern.
- **Refactoring.Guru:** [Builder Pattern](https://refactoring.guru/design-patterns/builder).
- **Effective Java:** Builders vs. Telescoping Constructors.

## 📝 **Mini Task (Production)**

Refactor this `executeQuery` function using the **Builder Pattern**.

```ts
function executeQuery(table, fields, filter, limit) {
  /* ... */
}
```

**Mission:**

1. Create a `DbQuery` class and a `DbQueryBuilder`.
2. Implement fluent methods: `selectFields()`, `withFilter()`, and `withLimit()`.
3. Show how the client creates a query for "users" with a filter "age > 18" and a limit of 50.
