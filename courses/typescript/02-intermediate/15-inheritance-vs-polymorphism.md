# 15. Inheritance vs Polymorphism

## 🎯 Learning Goal

Use classes interchangeably.

## 🧠 Concept

**Polymorphism**: Treating derived objects as their base class.
In TS, classes are compared **Structurally** (Duck Typing), not just nominatively.

## 💻 Implementation

```typescript
class Dog {
  bark() {}
}
class Wolf {
  bark() {}
}

let d: Dog = new Dog();
d = new Wolf(); // VALID! They have the same structure.
```

If you add a private member, structural compatibility breaks unless they share the origin.

## 🧩 Activity / Challenge

1.  Understand that TS is "Structurally Typed". Java/C# are "Nominally Typed".

## 🔑 Key Takeaways

- Don't over-engineer inheritance hierarchies. Interfaces are often better for polymorphism.
