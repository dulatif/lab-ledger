# 💡 SR03.2: Remove Middle Man

**Outline:**

- **The Smell (SEEI):** Identifying the "Middle Man" class, which does nothing but delegate work.
- **The Refactor (PPP):** Applying the "Remove Middle Man" technique to simplify client calls.
- **Your Refactoring Mission (Production):** Time to bypass a pointless delegator.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Smell (SEEI)**

**The Code Smell: Middle Man**
This is the opposite of "Hide Delegate." A Middle Man occurs when a class has been over-refactored to the point where it does very little on its own. Most methods are just one-line delegations to another object. If a `Person` class just forwards dozens of calls to a `Department` object without adding value, it has become a Middle Man.

**The Negative Impact**

- **Unnecessary Complexity:** Adds a layer of indirection for zero benefit, making code harder to navigate.
- **Maintenance Burden:** Every new method added to the target necessitates a redundant wrapper in the Middle Man (violating DRY).
- **Code Bloat:** Extra files and boilerplate that contribute no actual business logic.

**Example of the Smell**
A `Person` class that exists solely to forward calls to a `Manager`'s properties.

```ts
// SMELL: Person is a Middle Man.
class Person {
  public getManagerName() {
    return this.dept.getManager().getName();
  }
  public getManagerId() {
    return this.dept.getManager().getId();
  }
}
```

### **Part 2: Practice - The Refactor (PPP)**

**The Technique: Remove Middle Man**
Remove the delegating class and have the client call the real object directly.

1. **Identify** the real object (the delegate) doing the work (e.g., `Manager`).
2. **Expose:** Create a getter in the server class (`Person`) to return that delegate.
3. **Bypass:** For each delegating method, find callers and change them to access the delegate directly.
4. **Delete:** Remove the hollow wrapper methods from the Middle Man class.

**Key Question:** Does the server class do anything _other than_ delegate? If not, it's a Middle Man.

## 🧠 **Real-World Case Study: "Before vs. After Refactoring"**

- **Before (Middle Man):**
  Client code calls `john.getManagerName()`. To add `getManagerEmail`, you must edit `Person`, even though `Person` doesn't care about emails.
- **After (Remove Middle Man):**

  ```ts
  class Person {
    public getDepartment() {
      return this.dept;
    }
  }

  // Client talks to the department directly
  const manager = john.getDepartment().getManager();
  const name = manager.getName();
  ```

- **The Improvement:** Better honesty. The client code now explicitly shows its dependency on `Department`. `Person` is no longer a bottleneck for adding new properties to the manager. "Middle-logic" bloat is eliminated.

## 🤔 **Reflective Questions**

1. How do you decide between "Hide Delegate" and "Remove Middle Man"?
2. What does it mean for a class to "add value"?
3. Why is "honesty about dependencies" sometimes better than "strict encapsulation"?

## 📚 **Further Reading**

- **The Bible:** Martin Fowler's "Refactoring".
- **Refactoring.Guru:** [Remove Middle Man](https://refactoring.guru/remove-middle-man).

## 📝 **Mini Task (Production)**

Refactor this `UserProfile` Middle Man. The client should interact with the `auth` service directly.

```ts
class UserProfile {
  private auth: AuthenticationService;
  // SMELL: Pointless delegation
  public login(p: string) {
    return this.auth.login(p);
  }
}
```

**Mission:** Provide a way for the client to get the `AuthenticationService` (e.g., a getter) and remove the wrapper methods in `UserProfile`.
