# 18. Extra: Autowire Event classes

## 🎯 Learning Goal

Solve the problem of JSON deserialization losing type information.

## 🧠 Concept

When we save `new UserCreatedEvent('123')` to the DB, it becomes:
`{ "eventType": "UserCreatedEvent", "payload": { "userId": "123" } }`

When we load it back, it is a plain Object:
`{ userId: '123' }`

Crucially, `object instanceof UserCreatedEvent` is **FALSE**.
This breaks NestJS Event Handlers, which rely on `instanceof` to route events.

## 💻 Implementation

We need a **mapper** that knows how to reconstruct the class.

```typescript
const EventConstructors = {
  UserCreatedEvent: UserCreatedEvent,
  UserUpdatedEvent: UserUpdatedEvent,
};

function reconstruct(eventType: string, payload: any) {
  const ClassRef = EventConstructors[eventType];
  if (!ClassRef) return null;

  // Trick using Object.assign or class-transformer
  const instance = Object.create(ClassRef.prototype);
  return Object.assign(instance, payload);
}
```

## 🧩 Activity / Challenge

1.  Verify this yourself: `JSON.parse(JSON.stringify(new MyClass())) instanceof MyClass` is false.
2.  Implement a factory pattern to handle this rehydration.

## 🔑 Key Takeaways

- The Infrastructure Layer is responsible for this serialization/deserialization magic. The Domain just sees Objects.
