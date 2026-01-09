# Lesson 7: RxJS Basics

## Learning Goals

- Understand Observables vs Promises.
- Subscribe and Unsubscribe.

## 1. What is an Observable?

A Promise resolves **once**. An Observable emits values **over time** (a stream).

- Mouse Clicks.
- WebSocket messages.
- HTTP Responses.

## 2. Using Observables

```typescript
const stream$ = new Observable((observer) => {
  observer.next("Value 1");
  setTimeout(() => observer.next("Value 2"), 1000);
});

// Nothing happens yet (Cold)
const sub = stream$.subscribe((val) => console.log(val));
```

## 3. Memory Leaks

If you subscribe manually, you **must unsubscribe** manually (usually in `ngOnDestroy`), otherwise the subscription lives forever.

```typescript
ngOnDestroy() {
  this.sub.unsubscribe();
}
```

## Key Takeaways

- **$ Suffix**: Naming convention for observables (e.g., `users$`).
- **Streams**: Thinking in streams allows powerful composition.
