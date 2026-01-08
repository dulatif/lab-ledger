# 22. Real-time Updates with Subscriptions

## 🎯 Learning Goal

Push updates to the client.

## 🧠 Concept

Uses **WebSockets**.
Requires `installSubscriptionHandlers: true` in `GraphQLModule`.
Uses `PubSub` engine.

## 💻 Implementation

```typescript
const pubSub = new PubSub();

@Resolver()
export class ... {
  @Mutation()
  create() {
    // ...
    pubSub.publish('coffeeAdded', { coffeeAdded: newCoffee });
  }

  @Subscription(returns => Coffee)
  coffeeAdded() {
    return pubSub.asyncIterator('coffeeAdded');
  }
}
```

## 🧩 Activity / Challenge

1.  Open two Playground tabs.
2.  Tab 1: `subscription { coffeeAdded { name } }` (It hangs, waiting).
3.  Tab 2: Mutation Create.
4.  Tab 1: Receives data!

## 🔑 Key Takeaways

- In production, replace in-memory `PubSub` with Redis (`graphql-redis-subscriptions`).
