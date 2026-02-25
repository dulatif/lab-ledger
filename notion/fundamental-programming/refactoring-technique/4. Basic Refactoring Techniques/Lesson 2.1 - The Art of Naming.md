# 💡 BRT02.1: The Art of Naming

**Outline:**

- **The Smell (SEEI):** Identifying and understanding "Uninformative Names."
- **The "Refactor" (PPP):** Applying core principles for effective naming.
- **Your Refactoring Mission (Production):** Time to rename some confusing code.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Smell (SEEI)**

**The Code Smell: Uninformative Names (Obfuscated Code)**
This is the most fundamental smell. Names for variables, functions, or classes that don't reveal their purpose (e.g., `data`, `list`, `item`, `x`, `processData`). They force the reader to act as a detective to understand what the code actually does.

**The Negative Impact**

- **High Cognitive Load:** Mentally mapping meaningless names to their true purpose is exhausting and error-prone.
- **Bugs are Inevitable:** Misunderstanding a variable like `data` leads to incorrect usage.
- **Hard Technical Onboarding:** New team members can't understand code full of `tmp`, `val`, and `mgr`.
- **Blocks Refactoring:** You can't safely extract methods if you don't understand what the data represents.

**Example of the Smell**
A short but inscrutable function.

```ts
function check(list: any[], d: number): any[] {
  const res = [];
  for (const x of list) {
    if (x.e && x.t > d) {
      res.push(x);
    }
  }
  return res;
}
```

### **Part 2: Practice - The Refactor (PPP)**

**The Technique: Naming with Intention**
Following core principles to guide you:

1. **Intention-Revealing:** The name tells why it exists and what it does (e.g., `elapsedTimeInDays`).
2. **Pronounceable:** If you can't say it in a discussion, it's bad (e.g., `genTimestamp`).
3. **Searchable:** Avoid single letters; use names like `MAX_STUDENTS`.
4. **Avoid Encodings:** No prefixes like `strName` or `m_name`.
5. **Consistent:** Use the same vocabulary (e.g., don't mix `fetch`, `retrieve`, and `get` for the same concept).

**Step-by-Step Implementation**
Refactoring the `check` function:

1. `list` -> `users`
2. `d` -> `timestampThreshold`
3. `x` -> `user`
4. `e` -> `isEnabled`
5. `t` -> `creationTimestamp`
6. `check` -> `getActiveUsersSince`

## 🧠 **Real-World Case Study: "Before vs. After Refactoring"**

- **Before (Uninformative):**
  Uses `list`, `d`, `res`, `x.e`, `x.t`. Completely opaque without a manual trace.
- **After (Intention-Revealing):**
  ```ts
  function getActiveUsersSince(
    users: User[],
    timestampThreshold: number,
  ): User[] {
    return users.filter(
      (user) => user.isEnabled && user.creationTimestamp > timestampThreshold,
    );
  }
  ```
- **The Improvement:** Code becomes self-documenting prose. The purpose and parameters are immediately clear, reducing the chance of misuse to near zero.

## 🤔 **Reflective Questions**

1. What did Robert C. Martin mean by names "answering big questions"?
2. When are single-letter variable names actually acceptable?
3. How can a team enforce consistent naming across a large project?

## 📚 **Further Reading**

- **Clean Code:** Chapter 2 (Meaningful Names) by Robert C. Martin.
- **Naming Cheatsheet:** [Link](https://github.com/kettanaito/naming-cheatsheet).
- **Airbnb Style Guide:** Sections on naming conventions.

## 📝 **Mini Task (Production)**

Refactor this `calc` function by giving everything an intention-revealing name.

```ts
function calc(p: any, c: boolean): number {
  const dis = c ? 0.1 : 0; // discount
  const f = p.prc - p.prc * dis;
  const tax = f * p.tax_rt;
  const ship = p.wgt < 10 ? 5 : 15;

  return f + tax + ship;
}
```

**Variables to fix:** `p` (product), `c` (isCustomerPremium), `dis` (discountRate), `f` (priceAfterDiscount), `tax_rt` (taxRate), `wgt` (weight).
