# 💡 ACS01.2: The God Object Antipattern

**Outline:**

- **The Smell (SEEI):** Understanding the "God Object," a super-sized Large Class that controls everything.
- **The Refactor (PPP):** Applying the "Extract Class" technique to delegate responsibilities.
- **Your Refactoring Mission (Production):** Time to de-throne a God Object.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Smell (SEEI)**

**The Code Smell: God Object**
A **God Object** is a particularly nasty variant of the Large Class. It's not just big; it's a class that knows too much and does too much, often acting as the central controller for a huge portion of the application. It holds a large amount of the program's state and has methods for manipulating a wide range of other, smaller objects.

**The Negative Impact: The Single Point of Failure**
God Objects create systems that are extremely brittle and resistant to change:

- **Massive Coupling:** Hundreds of other classes are directly coupled to it. A change to the God Object can have unpredictable ripple effects.
- **No Clear Boundaries:** It's impossible to understand one feature without understanding the entire God Object.
- **Concurrency Issues:** Because it holds so much state, it becomes a major point of contention in multi-threaded environments.
- **Effectively Untestable:** The sheer number of dependencies and states makes it practically impossible to unit test.

**Example of the Smell: The `GameManager`**
In a game, a `GameManager` might grow into a God Object that controls everything.

```ts
class GameManager {
  public player: Player;
  public enemies: Enemy[];
  public level: Level;
  public ui: UI;
  public audio: AudioManager;

  // Player logic
  public movePlayer(x: number, y: number) {
    /* ... */
  }
  public playerAttack() {
    /* ... */
  }

  // Enemy logic
  public spawnEnemies() {
    /* ... */
  }
  public updateEnemyAI() {
    /* ... */
  }

  // Level logic
  public loadLevel(levelName: string) {
    /* ... */
  }

  // UI logic
  public updateScoreboard() {
    /* ... */
  }

  // Game state logic
  public checkWinCondition() {
    /* ... */
  }

  // Main game loop
  public update() {
    this.updateEnemyAI();
    this.updateScoreboard();
    this.checkWinCondition();
  }
}
```

Other classes (Player, Enemy, UI) likely have no logic of their own; they are just manipulated by the `GameManager`.

### **Part 2: Practice - The Refactor (PPP)**

**The Technique: Extract Class**
The primary weapon against God Objects is the **"Extract Class"** refactoring. This is the practical application of Responsibility Mapping.

1. **Identify a Cohesive Responsibility:** (e.g., "Enemy Management").
2. **Create a New Class:** (e.g., `EnemyManager`).
3. **Move Methods and Fields:** Move relevant methods and data to the new class.
4. **Delegate:** The God Object no longer handles the logic; it holds an instance of the new class and _delegates_ calls to it.

**Step-by-Step Implementation: Breaking Down the `GameManager`**
Let's extract the "Enemy Management" responsibility.

```ts
// Step 2: Create a new, focused class
class EnemyManager {
  private enemies: Enemy[];
  private player: Player;

  constructor(enemies: Enemy[], player: Player) {
    this.enemies = enemies;
    this.player = player;
  }

  // Step 3: Move the relevant methods
  public spawnEnemies() {
    /* ... */
  }
  public updateEnemyAI() {
    /* ... */
  }
}

// The God Object becomes smaller and delegates responsibility
class GameManager {
  public player: Player;
  private enemyManager: EnemyManager;

  constructor() {
    this.player = new Player();
    this.enemyManager = new EnemyManager([], this.player);
  }

  // Step 4: Delegate the calls
  public spawnEnemies() {
    this.enemyManager.spawnEnemies();
  }

  public updateEnemyAI() {
    this.enemyManager.updateEnemyAI();
  }
}
```

## 🧠 **Real-World Case Study: "Before vs. After Refactoring"**

- **Before (God Object):**
  ```ts
  class GameManager {
    public enemies: Enemy[];
    public updateEnemyAI() {
      /* complex logic here */
    }
  }
  ```
- **After (Delegation):**
  ```ts
  class EnemyManager {
    public updateEnemyAI() {
      /* complex logic here */
    }
  }
  class GameManager {
    private enemyManager: EnemyManager;
    public updateEnemyAI() {
      this.enemyManager.updateEnemyAI();
    }
  }
  ```
- **The Improvement:**
  - **Increased Cohesion:** `EnemyManager` is highly focused.
  - **Reduced Coupling:** `GameManager` no longer knows internal AI details.
  - **Improved Testability:** Unit test `EnemyManager` independently.
  - **Clear Boundaries:** Bug in enemies? Look in `EnemyManager.ts`.

## 🤔 **Reflective Questions**

1. How is a God Object different from a Facade design pattern?
2. What would be the next logical class to extract from our `GameManager` example?
3. How does Dependency Injection make the `GameManager` even more flexible here?

## 📚 **Further Reading**

- **The Bible:** Fowler's "Refactoring," see the "Extract Class" technique.
- **Antipatterns:** [God Object on Wikipedia](https://en.wikipedia.org/wiki/God_object).
- **SOLID Principles:** God Objects violate every single SOLID principle.

## 📝 **Mini Task (Production)**

Extract the **authentication responsibility** from the `MainController` below.

**Your Mission:**

1. Create a new `AuthService` class.
2. Move authentication methods (`login`, `logout`, `checkStatus`) and properties (`currentUser`).
3. Delegate authentication calls from `MainController` to `AuthService`.

```ts
class MainController {
  public currentUser: User | null = null;
  public data: any[] = [];

  public login(user: string, pass: string): boolean {
    console.log("Authenticating user...");
    if (user === "admin") {
      this.currentUser = { name: "admin" };
      return true;
    }
    return false;
  }
  public logout(): void {
    this.currentUser = null;
  }
  public checkStatus(): boolean {
    return !!this.currentUser;
  }

  public loadData(): void {
    console.log("Fetching data...");
    this.data = [{ id: 1, value: "A" }];
  }

  public render(): string {
    if (!this.checkStatus()) return "<h1>Please log in</h1>";
    return `<div>Welcome ${this.currentUser?.name}</div>`;
  }
}
```
