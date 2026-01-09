# Lesson 1: Signals Architecture

## Learning Goals

- Understand Signals (Angular 16+).
- Use `signal`, `computed`, and `effect`.

## 1. What is a Signal?

A wrapper around a value that can notify interested consumers when that value changes.
Unlike Observables, they are synchronous and glitter-free.

```typescript
count = signal(0); // WritableSignal<number>

// Read
console.log(this.count());

// Write
this.count.set(5);
this.count.update((c) => c + 1);
```

## 2. Computed

Derive state from other signals. It updates automatically and lazily.

```typescript
doubleCount = computed(() => this.count() * 2);
```

## 3. Inputs as Signals (Angular 17.1+)

```typescript
// Child component
userId = input.required<string>(); // Signal<string>

// Effect (Side effect)
constructor() {
  effect(() => {
    console.log(`User ID changed to: ${this.userId()}`);
  });
}
```

## Key Takeaways

- **Signals** are the future of state in Angular.
- They enable **Zoneless** applications (removing Zone.js for better performance).
- Prefer Signals over `BehaviorSubject` for local synchronous state.
