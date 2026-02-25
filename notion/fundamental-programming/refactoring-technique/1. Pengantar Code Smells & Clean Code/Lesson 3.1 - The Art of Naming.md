# 💡 CC03.1: The Art of Naming - Intention-Revealing Names

**Outline:**

- **The Smell (SEEI):** Identifying "Disinformation" - names that are confusing, ambiguous, or just plain wrong.
- **The Refactor (PPP):** Applying the technique of choosing intention-revealing names.
- **Your Refactoring Mission (Production):** Time to rename and bring clarity to some code.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Smell (SEEI)**

**The Code Smell: Disinformation and Ambiguity**
This is one of the most subtle but pervasive smells. It occurs when names for variables, functions, or classes don't reveal their purpose. This includes single-letter variable names (`d` for elapsed time), vague names (`processData`), or names that are outright misleading (`userList` for an object that isn't a list/array). Bad names force the next developer to mentally translate the code, slowing down comprehension and increasing the chance of bugs.

**The Negative Impact: Why are bad names so costly?**
They create mental friction and hide the code's true intent:

- **Increased Cognitive Load:** You have to read the _implementation_ of a function to understand what it does, because its name is useless.
- **Bugs from Misunderstanding:** If a variable `d` means "elapsed time in days," but a developer assumes it means "distance," they will introduce a bug.
- **Harder to Search:** Searching for a variable named `u` or `item` in a large codebase is impossible.
- **Wasted Time:** The majority of a developer's time is spent reading code. Good names make reading fast and efficient.

**Example of the Smell: The Ambiguous `getThem` function**
What does this code do? You can't tell without reading every line carefully.

```ts
// What is "list"? What is "x"? What are "them"?
function getThem(list: any[][]): any[][] {
  const them: any[][] = [];
  for (const x of list) {
    if (x[0] === 4) {
      them.push(x);
    }
  }
  return them;
}
```

This code is a puzzle. The names provide no clues.

### **Part 2: Practice - The Refactor (PPP)**

**The Technique: Choose Intention-Revealing Names**
Choose names that clearly communicate what the thing is, what it does, and why it exists.

1. **Be Specific:** `activeUsers` is better than `users`. `elapsedTimeInDays` is better than `d`.
2. **Use Pronounceable Names:** A name you can say out loud is easier to discuss (`customer` vs. `cstmr`).
3. **Avoid Encodings:** Don't use Hungarian notation or member prefixes (`m_name`).
4. **For Functions, Use Verb/Noun Phrases:** `getActiveUsers()`, `calculateTotalPrice()`.

**Step-by-Step Implementation: Renaming `getThem`**
Let's apply these rules to our puzzle function. The code itself won't change, only the names.

```ts
// Let's assume this is a game board. A status of "4" means "flagged for review".
const FLAGGED_STATUS = 4;

interface Cell extends Array<number> {}
type GameBoard = Cell[];

function getFlaggedCells(gameBoard: GameBoard): GameBoard {
  const flaggedCells: GameBoard = [];
  for (const cell of gameBoard) {
    const cellStatus = cell[0];
    if (cellStatus === FLAGGED_STATUS) {
      flaggedCells.push(cell);
    }
  }
  return flaggedCells;
}
```

## 🧠 **Real-World Case Study: "Before vs. After Refactoring"**

- **Before (Ambiguous):**
  ```ts
  function getThem(list: any[][]): any[][] {
    const them: any[][] = [];
    for (const x of list) {
      if (x[0] === 4) {
        them.push(x);
      }
    }
    return them;
  }
  ```
- **After (Intention-Revealing):**

  ```ts
  function getFlaggedCells(gameBoard: GameBoard): GameBoard {
    const flaggedCells: GameBoard = [];
    for (const cell of gameBoard) {
      if (isFlagged(cell)) {
        flaggedCells.push(cell);
      }
    }
    return flaggedCells;
  }

  function isFlagged(cell: Cell): boolean {
    const FLAGGED_STATUS = 4;
    return cell[0] === FLAGGED_STATUS;
  }
  ```

- **The Improvement:**
  - **Instant Comprehension:** You understand `getFlaggedCells` just by reading its name.
  - **No "Magic Numbers":** `4` is now clearly defined as `FLAGGED_STATUS`.
  - **Enhanced Maintainability:** The code is now self-documenting.

## 🤔 **Reflective Questions**

1. Why is a descriptive name like `numberOfActiveUsersInCurrentSession` often better than `num`?
2. How can the context of a class help you choose shorter names without losing clarity?
3. When is it acceptable to use a single-letter variable name like `i`, `j`, `k`?

## 📚 **Further Reading**

- **The Bible:** "Clean Code" by Robert C. Martin has a foundational chapter called "Meaningful Names."
- **Code Quality Tool:** ESLint plugins like `eslint-plugin-unicorn` have rules to catch bad naming.
- **The Philosophy:** Read about making your code "tell a story."

## 📝 **Mini Task (Production)**

You are given a function with terrible names.

**Your Mission:**
Rename the variables and the function so the code's purpose is immediately clear. Do not change the logic.

```ts
// The function takes an array of transactions, a user object, and a limit.
// It returns true if the total of the user's recent transactions is over the limit.
function check(data: number[], obj: any, lim: number): boolean {
  let total = 0;
  for (let i = 0; i < data.length; i++) {
    total += data[i];
  }

  if (obj.isPremium) {
    return total > lim * 2;
  } else {
    return total > lim;
  }
}
```
