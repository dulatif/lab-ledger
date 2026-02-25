# 💡 SR03.1: Hide Delegate

**Outline:**

- **The Smell (SEEI):** Identifying "Message Chains" and their violation of the Law of Demeter.
- **The Refactor (PPP):** Applying the "Hide Delegate" technique to encapsulate internal object structures.
- **Your Refactoring Mission (Production):** Time to shorten a long message chain.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Smell (SEEI)**

**The Code Smell: Message Chain (Train Wreck)**
A message chain occurs when client code must navigate through a series of objects to get what it wants (e.g., `a.getB().getC().getName()`). This is a violation of the **Law of Demeter** (Principle of Least Knowledge): an object should only talk to its immediate "friends," not to "strangers."

**The Negative Impact**

- **High Coupling:** The client is coupled to the _entire structure_ of the object graph. Any internal change breaks the client.
- **Broken Encapsulation:** It forces the client to know exactly how objects are nested and related.
- **Testing Pain:** To mock one value, you must mock the entire string of objects.

**Example of the Smell**
A client navigating from a `Person` to a `Department` just to get a `Manager`'s name.

```ts
// SMELL: Coupled to Person, Department, and Manager classes.
const managerName = john.getDepartment().getManager().name;
```

### **Part 2: Practice - The Refactor (PPP)**

**The Technique: Hide Delegate**
Hiding the navigation inside the server object. The client asks for the final result, and the server handles the middle-men.

1. **Identify** what the client ultimately wants (e.g., the manager's name).
2. **Create** a new method on the first friend (`Person`) that encapsulates the chain.
3. **Encapsulate:** The client calls the new method, and the internal navigation becomes a private concern.

**Step-by-Step Implementation**

1. Create `Person.getManagerName()`.
2. Inside `Person`, return `this.department.getManager().name`.
3. Update the client to call `john.getManagerName()`.

## 🧠 **Real-World Case Study: "Before vs. After Refactoring"**

- **Before (Message Chain):**
  Client code is a "train wreck" of dots. If the `Department` structure changes, dozens of client files might break.
- **After (Hide Delegate):**
  ```ts
  class Person {
    // Client only talks to John
    public getManagerName() {
      return this.department.getManager().name;
    }
  }
  ```
- **The Improvement:** Encapsulation is restored. The client doesn't know `Department` or `Manager` even exist. Any structural changes only require one update inside `Person`.

## 🤔 **Reflective Questions**

1. What is the Law of Demeter?
2. How does "Hide Delegate" protect your code from structural shifts in the object graph?
3. When can "Hide Delegate" be overused? (Hint: creating hundreds of wrapper methods).

## 📚 **Further Reading**

- **The Bible:** Martin Fowler's "Refactoring".
- **Refactoring.Guru:** [Hide Delegate](https://refactoring.guru/hide-delegate).
- **Principle:** [Law of Demeter](https://en.wikipedia.org/wiki/Law_of_Demeter).

## 📝 **Mini Task (Production)**

Refactor this message chain in a file system simulation.

```ts
// SMELL: Message chain
const fileExtension = fs.getRootFile().getType().getExtension();
```

**Mission:** Create a `getRootFileExtension()` method in the `Filesystem` class that hides the inner data navigation from the client.
