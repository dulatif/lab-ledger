# 3. Intersection Types vs Interface Inheritance

## 🎯 Learning Goal

Know when to use `extend` vs `&`.

## 🧠 Concept

**Inheritance (`extends`)**: Hierarchical. Must be compatible.
**Intersection (`&`)**: Compositional. Combines members.

## 💻 Implementation

### Intersection

```typescript
type Draggable = { drag: () => void };
type Resizable = { resize: () => void };

// A UI widget is BOTH
type UIWidget = Draggable & Resizable;

const widget: UIWidget = {
  drag: () => {},
  resize: () => {},
};
```

### Handling Conflicts

- **Interface**: Errors if you try to extend with a conflicting type (e.g., `name: string` vs `name: number`).
- **Intersection**: Creates `never` for the conflicting field (it can't be both string and number).

## 🧩 Activity / Challenge

1.  Combine `Admin` and `User` types using Intersection.
2.  Try to intersect primitive types (`string & number`) and check the result type (`never`).

## 🔑 Key Takeaways

- Use `extends` for object hierarchies.
- Use `&` for ephemeral combinations or mixins.


Next: [[04-types-vs-interfaces]]