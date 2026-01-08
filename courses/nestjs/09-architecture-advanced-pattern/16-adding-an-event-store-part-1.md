# 16. Adding an Event Store. Part 1

## 🎯 Learning Goal

Design the interface for our Event Store.

## 🧠 Concept

The NestJS `EventPublisher` usually just emits events to handlers. But in Event Sourcing, we must **Store** them first, then emit them.
We need a custom Publisher called an `EventStore`.

## 💻 Implementation

### 1. The Interface

```typescript
export interface IEventStore {
  save(aggregateId: string, events: any[]): Promise<void>;
  getEvents(aggregateId: string): Promise<any[]>;
}
```

### 2. The Bridge

We need to hook into the command handler.
Instead of `repository.save(user)`, we will do `eventStore.save(user.id, user.getUncommittedEvents())`.

## 🧩 Activity / Challenge

1.  Imagine the DB schema for an Event Store.
    - `aggregateId` (index)
    - `version` (sequence number)
    - `eventType` (string)
    - `payload` (json)
    - `timestamp`

## 🔑 Key Takeaways

- The Event Store is a generic database. It doesn't know about "Users" or "Orders". It only knows "Streams of JSON".
