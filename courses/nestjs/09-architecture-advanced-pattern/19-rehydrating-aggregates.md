# 19. Rehydrating Aggregates

## 🎯 Learning Goal

Reconstruct the state of an Aggregate by replaying its history.

## 🧠 Concept

To process a command (e.g., "Change Name"), we need the current User state.

1.  Fetch all events for `User:123`.
2.  Create empty `User`.
3.  Apply `UserCreated`.
4.  Apply `UserRenamed`.
5.  Apply `UserAddressChanged`.
6.  Now we have the **Current State**.

## 💻 Implementation

Decorate methods inside the Aggregate with `@AggregateEventHandler` (not a real Nest decorator, conceptual).
Usually, NestJS Aggregates have a `loadFromHistory(events)` method.

```typescript
class User extends AggregateRoot {
  loadFromHistory(history: any[]) {
    history.forEach((event) => this.apply(event, true)); // true = isFromHistory
  }

  // State transitions
  onUserCreatedEvent(event) {
    this.id = event.id;
  }
  onUserRenamedEvent(event) {
    this.name = event.name;
  }
}
```

## 🧩 Activity / Challenge

1.  Update your `UserRepository`.
2.  `findOne(id)` should now calling `eventStore.getEvents(id)` and then `user.loadFromHistory(events)`.

## 🔑 Key Takeaways

- The logic to **Calculate State** lives inside the Aggregate.
- The DB only stores the **Ingredients** (Events), not the **Cake** (State).
