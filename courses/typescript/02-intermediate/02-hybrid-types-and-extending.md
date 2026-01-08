# 2. Hybrid Types and Extending Interfaces

## 🎯 Learning Goal

Compose complex types from simpler ones.

## 🧠 Concept

Interfaces can **extend** one or more other interfaces, copying their members.
This allows you to build composite types without duplication.

## 💻 Implementation

### Extending

```typescript
interface Animal {
  name: string;
}

interface Dog extends Animal {
  breed: string;
}

const myDog: Dog = { name: "Rex", breed: "German Shepherd" };
```

### Hybrid Types (Function + Object)

In JavaScript, functions are objects. They can have properties.

```typescript
interface Counter {
  (start: number): string; // Call signature
  interval: number; // Property
  reset(): void; // Method
}

function getCounter(): Counter {
  let counter = function (start: number) {} as Counter;
  counter.interval = 123;
  counter.reset = function () {};
  return counter;
}
```

## 🧩 Activity / Challenge

1.  Create `Vehicle`, `Car` (extends Vehicle), and `ElectricCar` (extends Car).
2.  Add properties specific to each level.

## 🔑 Key Takeaways

- `extends` promotes reusability.
- Interfaces can model weird JS patterns like "Functions with properties".
