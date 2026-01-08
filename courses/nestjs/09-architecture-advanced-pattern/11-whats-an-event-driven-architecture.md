# 11. What's an Event-Driven Architecture?

## 🎯 Learning Goal

Transition from "Orchestration" (Service A calls Service B) to "Choreography" (Service A shouts "I'm done", Service B reacts).

## 🧠 Concept

When a user is registered, we typically want to:

1.  Send a Welcome Email.
2.  Create a default Wallet.
3.  Notify Admins.

**Tightly Coupled**: The `RegisterUserService` calls `EmailService.send`, `WalletService.create`...
**Event Driven**: The `RegisterUserService` simply publishes `UserRegisteredEvent`.
The Email System listens to it. The Wallet System listens to it.

## 💻 Implementation

In DDD, an Aggregate Root publishes events.

```typescript
class User extends AggregateRoot {
  register() {
    // ... logic
    this.apply(new UserRegisteredEvent(this.id)); // 📣 Shout it out!
  }
}
```

NestJS CQRS module handles dispatching these events when you "commit" the aggregate.

## 🧩 Activity / Challenge

1.  Think about "Side Effects" in your current app.
2.  Are they blocking the main request? (e.g., waiting for email SMTP).
3.  Events allow these to run asynchronously and independently.

## 🔑 Key Takeaways

- **Events** describe something that _has happened_ (Past Tense). `UserRegistered`, not `RegisterUser`.
- **Fire and Forget**: The command handler doesn't wait for the event handlers to finish.
