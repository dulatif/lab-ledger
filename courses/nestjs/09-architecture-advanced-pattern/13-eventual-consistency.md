# 13. Eventual Consistency

## 🎯 Learning Goal

Accept the reality that in distributed systems, data might not be up-to-date instantly everywhere.

## 🧠 Concept

In a monolithic CRUD app with one DB transaction, everything is **Atomic**. You save the user, and if you read the DB 1ms later, the user is there.

In CQRS/Event-Driven:

1.  Command Handler saves to Write DB.
2.  Event is published.
3.  (500ms delay) Query Handler updates the Read DB (Projection).

If the user tries to "Get Profile" immediately after "Register", they might get 404! 😱
This is **Eventual Consistency**. The system _will_ be consistent... eventually.

## 💻 Implementation

Strategies to deal with it:

1.  **UI Optimistic Updates**: Assume success and show the user the new state before the server confirms.
2.  **Versioning**: Client sends "I expect version 5". Server waits until version 5 is ready.
3.  **Acceptance**: "Your request is processing".

## 🧩 Activity / Challenge

1.  This is a theoretical concept, but crucial for the next lesson.
2.  Understand that "Strong Consistency" (ACID) is expensive and slow at scale. "Eventual Consistency" (BASE) is fast and scalable.

## 🔑 Key Takeaways

- **ACID**: Atomicity, Consistency, Isolation, Durability.
- **BASE**: Basic Availability, Soft-state, Eventual consistency.
