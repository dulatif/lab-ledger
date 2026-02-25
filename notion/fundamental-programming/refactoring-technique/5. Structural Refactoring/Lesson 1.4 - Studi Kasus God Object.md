# 💡 SR01.4: Studi Kasus God Object

**Outline:**

- **The Smell (SEEI):** Identifying the "God Object"—the ultimate example of low cohesion and too many responsibilities.
- **The Refactor (PPP):** Applying a strategic, step-by-step process of decomposition using `Extract Class` and `Move Method`.
- **Your Refactoring Mission (Production):** Time to plan the decomposition of a real-world God Object.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Smell (SEEI)**

**The Code Smell: The God Object**
A God Object is the extreme form of "Large Class." Over time, it accumulates dozens of unrelated responsibilities, becoming the center of the universe for your application. Everyone depends on it, and it depends on everyone.

**The Negative Impact**

- **SRP Overload:** Often has 5-10+ unrelated responsibilities.
- **Minimum Cohesion:** Methods and properties belong to distinct, unrelated clusters.
- **Coupling Nightmare:** Coupling is maximized, making the system fragile.
- **Untestable:** Setting up the object's state requires massive mocking and configuration.
- **Big Ball of Mud:** No clear boundaries or separation of concerns.

**Example of the Smell**
A `GameManager` handling player state, rendering, sound, and saving.

```ts
class GameManager {
  private playerHealth: number; // Player
  private screenHeight: number; // Rendering
  private masterVolume: number; // Sound
  private saveFile: string; // Persistence

  public drawPlayer() {
    /* Rendering logic */
  }
  public playSound() {
    /* Audio logic */
  }
}
```

### **Part 2: Practice - The Refactor (PPP)**

**The Technique: Strategic Decomposition**
Fixing a God Object is an iterative game of "chipping away."

1. **Identify Responsibilities:** Cluster methods and fields (e.g., `Renderer`, `AudioManager`).
2. **Extract One at a Time:** Use **Extract Class** for one cluster (e.g., move rendering math to a `Renderer` class).
3. **Establish Relationship:** Give the God Object an instance of the new class.
4. **Delegate:** Replace logic in the God Object with one-line calls (e.g., `renderer.draw()`).
5. **Test & Repeat:** Ensure behavior is identical, then move to the next cluster.

## 🧠 **Real-World Case Study: "Before vs. After Refactoring"**

- **Before (God Object):**
  `GameManager` is a 2000-line file that developers are afraid to touch. Every change risks breaking something unrelated.
- **After (Coordinator & Specialists):**

  ```ts
  class GameManager {
    private player: Player;
    private renderer: Renderer;
    private audio: AudioService;

    public update() {
      this.player.update();
      this.renderer.render(this.player);
    }
  }
  ```

- **The Improvement:** `GameManager` becomes a clean **Coordinator** (or Facade). Specialist logic is encapsulated in small, testable classes. Adding a "Network" feature now involves a new `NetworkService` rather than bloated code in the manager.

## 🤔 **Reflective Questions**

1. Why are tests mandatory before touching a God Object?
2. How is a "Coordinator" different from a "God Object"?
3. What are the business risks of _not_ fixing a God Object? (e.g., technical debt, developer burnout).

## 📚 **Further Reading**

- **Working Effectively with Legacy Code:** Michael Feathers.
- **Anti-Patterns:** [God Object](https://en.wikipedia.org/wiki/God_object).
- **Design Patterns:** [Facade Pattern](https://refactoring.guru/design-patterns/facade).

## 📝 **Mini Task (Production)**

Plan the decomposition of this `Product` God Object. It handles data, JSON for API, and HTML for UI.

```ts
class Product {
  private name: string;
  private price: number;

  public toJSON() {
    /* Serialization logic... */
  }
  public toHtmlDiv() {
    /* Rendering logic... */
  }
}
```

**Mission:**

1. Name the two specialist classes you would extract.
2. List which methods would move to each.
3. Sketch the final, simplified `Product` class.
