# Lesson 8: RxJS Operators

## Learning Goals

- Transform data with `pipe()`.
- Common operators: `map`, `filter`, `tap`, `switchMap`.

## 1. Pipe Flow

Input Stream -> Operator 1 -> Operator 2 -> Output Stream.

```typescript
myStream$
  .pipe(
    filter((num) => num > 0), // Only positive
    map((num) => num * 2), // Double it
    tap((val) => console.log("Side effect", val)) // Log it
  )
  .subscribe();
```

## 2. Flattening Operators (switchMap)

Common Scenario: Search Box.
When user types 'A', trigger API.
User types 'B' immediately. Cancel 'A' API call, trigger 'B'.

```typescript
searchControl.valueChanges.pipe(
  debounceTime(300), // Wait for pause in typing
  switchMap(term => this.api.search(term)) // Switch to new inner observable
).subscribe(results => ...);
```

## Key Takeaways

- **Pipeable Operators**: The bread and butter of RxJS.
- **Marble Diagrams**: A visual way to understand operators.
- **SwitchMap**: Crucial for HTTP requests to avoid race conditions.
