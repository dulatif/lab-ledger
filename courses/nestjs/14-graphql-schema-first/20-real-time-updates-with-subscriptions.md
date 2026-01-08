# 20. Real-time Updates with Subscriptions

## 🎯 Learning Goal

Subscriptions in SDL.

## 🧠 Concept

SDL:

```graphql
type Subscription {
  coffeeAdded: Coffee!
}
```

## 💻 Implementation

Resolver:

```typescript
@Subscription() // Autodetects 'coffeeAdded' from method name
coffeeAdded() {
  return pubSub.asyncIterator('coffeeAdded');
}
```

## 🧩 Activity / Challenge

1.  Add Subscription type to SDL.
2.  Implement Resolver.
3.  Test with 2 tabs.

## 🔑 Key Takeaways

- The logic is identical to Code First. Only the definition location changed.
