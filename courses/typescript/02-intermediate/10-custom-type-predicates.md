# 10. Custom Type Predicates (User-Defined Guards)

## 🎯 Learning Goal

Teach TypeScript how to verify your custom interfaces.

## 🧠 Concept

`typeof` doesn't work for Interfaces (they don't exist at runtime).
We need a function that returns `parameter is Type`.

## 💻 Implementation

```typescript
interface Fish {
  swim: () => void;
}
interface Bird {
  fly: () => void;
}

// The Predicate: pet is Fish
function isFish(pet: Fish | Bird): pet is Fish {
  return (pet as Fish).swim !== undefined;
}

function move(pet: Fish | Bird) {
  if (isFish(pet)) {
    pet.swim(); // TS knows it's Fish
  } else {
    pet.fly(); // TS knows it's Bird
  }
}
```

## 🧩 Activity / Challenge

1.  Define an interface `Admin` with `role: 'admin'`.
2.  Write an `isAdmin(user: User): user is Admin` function.
3.  Use it in a filter array: `const admins = users.filter(isAdmin)`.

## 🔑 Key Takeaways

- Vital for filtering arrays where type information is lost.
