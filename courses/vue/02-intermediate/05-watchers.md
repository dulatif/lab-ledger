# Lesson 5: Watchers

## Learning Goals

- `watch` vs `watchEffect`.
- Side Effects.

## 1. watch()

Lazy. Only runs when the specific source changes.
Previous and Current values are available.

```typescript
const count = ref(0);

watch(count, (newVal, oldVal) => {
  console.log(`Changed from ${oldVal} to ${newVal}`);
  // Perform API call or complicated logic
});
```

## 2. watchEffect()

Immediate. Runs immediately, tracks dependencies automatically, and re-runs when ANY dependency changes.

```typescript
watchEffect(() => {
  // Runs now.
  // And runs whenever `count.value` changes because it is used inside.
  console.log(count.value);
});
```

## 3. Usage

- Use `watch` when you need specific control over **what** triggers the effect, or need the old value.
- Use `watchEffect` for simpler logic where you just want to "react to everything used here".

## Key Takeaways

- Watchers are for **Side Effects** (Async operations, DOM mutation).
- Use `computed` for calculating values.
