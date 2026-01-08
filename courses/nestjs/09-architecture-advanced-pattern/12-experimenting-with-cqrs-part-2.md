# 12. Experimenting with CQRS. Part 2

## 🎯 Learning Goal

Implement **Events** and **Event Handlers** to react to our Command.

## 🧠 Concept

We will modify our Command Handler to:

1.  Create a User Aggregate.
2.  Call a method that applies an event.
3.  Commit/Publish the event.

Then we create a separate Handler to listen.

## 💻 Implementation

### 1. The Event

```typescript
export class UserCreatedEvent {
  constructor(public readonly userId: string) {}
}
```

### 2. The Aggregate

```typescript
class User extends AggregateRoot {
  constructor(private id: string) {
    super();
  }

  created() {
    this.apply(new UserCreatedEvent(this.id));
  }
}
```

### 3. The Command Handler Update

```typescript
const user = new User("123");
user.created();
user.commit(); // 🚀 Dispatches events
```

### 4. The Event Handler

```typescript
@EventsHandler(UserCreatedEvent)
export class SendWelcomeEmailHandler
  implements IEventHandler<UserCreatedEvent>
{
  handle(event: UserCreatedEvent) {
    console.log(`📧 Sending email to user ${event.userId}`);
  }
}
```

## 🧩 Activity / Challenge

1.  Define `UserCreatedEvent`.
2.  Create `SendWelcomeEmailHandler`.
3.  Register it in `providers`.
4.  Run the command again. You should see "Creating user..." then separately "Sending email...".

## 🔑 Key Takeaways

- **Publisher/Subscriber**: The Command Handler is the publisher. The Event Handler is the subscriber.
- You can have **multiple** handlers for the same event (Email, Logging, Analytics).
