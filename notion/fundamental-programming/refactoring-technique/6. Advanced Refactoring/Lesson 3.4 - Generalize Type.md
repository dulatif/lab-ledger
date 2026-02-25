# 💡 AR03.4: Generalize Type

**Outline:**

- **The Smell (SEEI):** A class hierarchy where subclasses are nearly identical, differing only by the type of data they handle.
- **The Refactor (PPP):** Applying the "Generalize Type" refactoring by collapsing a type-based hierarchy into a single, reusable generic class.
- **Your Refactoring Mission (Production):** Time to generalize a hierarchy of data parsers.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Smell (SEEI)**

**The Code Smell: Type-Driven Hierarchy**
This occurs when you have an inheritance hierarchy where each subclass exists solely to handle a different data type. For example, `StringNode`, `NumberNode`, and `BooleanNode` that all share the exact same structural logic but differ only in their `value` property type.

**The Negative Impact**

- **Massive Code Duplication:** Structural logic (traversal, parenting, etc.) is repeated across every subclass.
- **Rigid and Unscalable:** Every new data type (e.g., `Date`) requires a whole new subclass and boilerplate.
- **Lost Abstraction:** The design stays low-level, missing the chance to capture the general concept of a "typed node."

**Example of the Smell**
A `ValueContainer` hierarchy with separate classes for strings and numbers that share 90% of the same code.

### **Part 2: Practice - The Refactor (PPP)**

**The Technique: Generalize Type (with Generics)**
Collapsing an entire type-based hierarchy into a single, generic class using `<T>`.

1. **Pick a Template:** Select one concrete subclass.
2. **Make it Generic:** Rename it to something general and add the `<T>` parameter (e.g., `ValueContainer<T>`).
3. **Swap Types:** Replace the specific type (`string`) with the type variable (`T`).
4. **Merge Parent Logic:** Copy all properties and methods from the original abstract superclass into this new generic class.
5. **Wipe Hierarchy:** Delete the old abstract class and all redundant subclasses.
6. **Update Callers:** Instantiate the generic class with the desired type: `new ValueContainer<string>()`.

## 🧠 **Real-World Case Study: "The Generic Container"**

- **Before (Type-Driven):**
  A developer has to maintain `StringValueContainer` and `NumberValueContainer`. Adding a `timestamp` field requires editing both files.
- **After (Single Generic Class):**
  ```ts
  class ValueContainer<T> {
    public readonly createdAt = new Date();
    constructor(private value: T) {}
    public getValue(): T {
      return this.value;
    }
  }
  ```
- **The Improvement:** Drastic simplification. One class does the work of many. It's now trivial to support `ValueContainer<User>` or `ValueContainer<Date>` without writing any new code.

## 🤔 **Reflective Questions**

1. How is this different from basic generic use? (Hint: it involves collapsing a hierarchy).
2. What if one subclass has a unique method the others don't?
3. How does this move relate to "Favor Composition over Inheritance"?

## 📚 **Further Reading**

- **TypeScript Doc:** [Generics](https://www.typescriptlang.org/docs/handbook/2/generics.html).
- **Book:** "Effective TypeScript" by Dan Vanderkam.

## 📝 **Mini Task (Production)**

Refactor this `Pair` hierarchy into a single generic `Pair<V>` class.

```ts
class StringPair extends Pair {
  constructor(
    key: string,
    public value: string,
  ) {
    super(key);
  }
}
```

**Mission:** Create the generic `Pair<V>` class where the key is always a `string`, but the value type is determined by the generic parameter. Show it in use for both strings and numbers.
