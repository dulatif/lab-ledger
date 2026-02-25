# 💡 AR02.3: Refactoring to Generics

**Outline:**

- **The Smell (SEEI):** Duplicated classes or functions that are identical except for the data types they handle.
- **The Refactor (PPP):** Applying "Generalize Type" by refactoring to use TypeScript Generics.
- **Your Refactoring Mission (Production):** Time to convert two duplicate functions into a single, reusable generic function.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Smell (SEEI)**

**The Code Smell: Type-Specific Duplication**
This occurs when you have multiple classes or functions that perform the exact same logic but operate on different concrete types. For example, a `StringFetcher` and a `NumberFetcher` that both handle caching and logging identically but return different types.

**The Negative Impact**

- **Maintenance Burden:** Any bug fix or update to the shared logic must be manually copied across all versions.
- **Code Bloat:** Unnecessary complexity and weight in the codebase.
- **Missed Abstraction:** Missing out on creating powerful, general-purpose components.

**Example of the Smell**
Two data stores, one for strings and one for numbers, with identical `add()` and `get()` logic.

```ts
class StringStore { data: string[]; add(i: string) { ... } }
class NumberStore { data: number[]; add(i: number) { ... } }
```

### **Part 2: Practice - The Refactor (PPP)**

**The Technique: Refactoring to Generics (Generalize Type)**
Generics allow you to write functions and classes that work with "type variables" rather than concrete types.

1. **Pick a Template:** Choose one of the duplicated classes.
2. **Add Type Parameter:** Add `<T>` to the class/function definition (e.g., `class DataStore<T>`).
3. **Replace Concrete Types:** Swap `string` or `number` with `T` throughout the logic.
4. **Wipe the Duplicates:** Delete the redundant versions.
5. **Update Callers:** Specify the type when instantiating (e.g., `new DataStore<string>()`).

## 🧠 **Real-World Case Study: "Before vs. After Refactoring"**

- **Before (Duplicated Classes):**
  A developer has to maintain `UserStorage`, `PaymentStorage`, and `OrderStorage`. If they want to add a `clear()` method, they must do it three times.
- **After (Single Generic Class):**

  ```ts
  class Storage<T> {
    private items: T[] = [];
    public add(item: T) {
      this.items.push(item);
    }
    public clear() {
      this.items = [];
    }
  }

  const userStorage = new Storage<User>();
  const orderStorage = new Storage<Order>();
  ```

- **The Improvement:** 100% DRY code. Adding a feature once enables it for all data types immediately. The system is more robust and scalable.

## 🤔 **Reflective Questions**

1. How do generics enforce type safety at compile time?
2. What is a "Generic Constraint" (e.g., `<T extends BaseEntity>`)?
3. Can you use generics in interfaces and stand-alone functions?

## 📚 **Further Reading**

- **TypeScript Handbook:** [Generics](https://www.typescriptlang.org/docs/handbook/2/generics.html).
- **Book:** "Programming with Types" by Vlad Riscutia.

## 📝 **Mini Task (Production)**

Refactor these two identical functions into a single generic function `findLast<T>`.

```ts
function findLastString(arr: string[]): string | undefined {
  return arr[arr.length - 1];
}

function findLastNumber(arr: number[]): number | undefined {
  return arr[arr.length - 1];
}
```

**Mission:** Create the generic `findLast` function and show how it works with both strings and a custom interface like `interface User { id: string }`.
