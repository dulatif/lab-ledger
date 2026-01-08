# 11. Request-Response vs Event-Driven

## 🎯 Learning Goal

Choose the right communication pattern.

## 🧠 Concept

**Request-Response (`client.send()`)**:

- "Hey Orders, create this." -> Waits... -> "Done, here is ID 5".
- Synchronous-ish. The caller waits for an answer.
- **Coupling**: High. If Orders receives it but crashes before replying, Caller fails.

**Event-Driven (`client.emit()`)**:

- "Hey everyone, OrderCreated!" -> Returns immediately.
- Asynchronous. Fire and forget.
- **Coupling**: Low. Caller assumes someone will handle it eventually.

## 💻 Implementation

### Send (Req/Res)

```typescript
// Returns an Observable that emits the result
client.send({ cmd: "sum" }, [1, 2, 3]);
```

### Emit (Event)

```typescript
// Returns an Observable that completes immediately (Hot)
client.emit("order_created", { id: 1 });
```

## 🧩 Activity / Challenge

1.  Change your Order creation to be Event Driven.
2.  The Gateway returns "202 Accepted" instantly.
3.  The Order Service processes it in the background.

## 🔑 Key Takeaways

- Use **Events** for notification / side-effects (Email, Analytics).
- Use **Request-Response** when you need data back immediately (Get User Profile).
