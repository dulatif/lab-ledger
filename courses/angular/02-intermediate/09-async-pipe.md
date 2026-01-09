# Lesson 9: Async Pipe

## Learning Goals

- Consume Observables in HTML.
- Avoid manual subscriptions.

## 1. The Problem

Manual `subscribe()` in `ngOnInit` requires `unsubscribe()` in `ngOnDestroy`. It is boilerplat-y and error-prone.

## 2. The Solution: | async

The `async` pipe subscribes automatically when the component loads, and **unsubscribes automatically** when it gets destroyed.

**TS**:

```typescript
users$ = this.http.get<User[]>("/api/users");
```

**HTML**:

```html
<ul>
  @for (user of users$ | async; track user.id) {
  <li>{{ user.name }}</li>
  }
</ul>
```

## 3. Loading/Error States

We can use structural patterns to handle loading.

```html
@if (users$ | async; as users) {
<!-- Show list -->
} @else {
<p>Loading...</p>
}
```

## Key Takeaways

- **Best Practice**: Use `async` pipe whenever possible.
- Avoid storing data in manual variables (`this.users = data`). Keep it as a stream (`users$`).
