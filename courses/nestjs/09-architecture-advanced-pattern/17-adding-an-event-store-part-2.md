# 17. Adding an Event Store. Part 2

## 🎯 Learning Goal

Implement a simple In-Memory Event Store.

## 🧠 Concept

We will use a specialized class to hold events and a method to push them.

## 💻 Implementation

```typescript
@Injectable()
export class EventStore {
  private readonly store: Map<string, any[]> = new Map();

  async save(aggregateId: string, events: any[]) {
    const stream = this.store.get(aggregateId) || [];
    const newStream = [...stream, ...events];
    this.store.set(aggregateId, newStream);

    // ⚠️ Crucial: After saving, we must publish them so Event Handlers run!
    events.forEach((event) => this.eventBus.publish(event));
  }

  async getEvents(aggregateId: string) {
    return this.store.get(aggregateId) || [];
  }
}
```

## 🧩 Activity / Challenge

1.  Implement this logic.
2.  Note the **Concurrency Problem**: What if two commands try to append to the same stream at the same time?
    - Solution: Optimistic Concurrency Control using a `version` number. If the DB version doesn't match the expected version, throw `ConcurrencyException`.

## 🔑 Key Takeaways

- **Save -> Publish**. If Save fails, Publish never happens. Consistency guaranteed.
- **Streams**: Each Aggregate Instance (User:1, User:2) has its own stream of events.
