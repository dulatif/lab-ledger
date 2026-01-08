# 9. Converting from HTTP to NATS

## 🎯 Learning Goal

Switch from point-to-point (TCP/HTTP) to **Broker-based** messaging.

## 🧠 Concept

**TCP**: Gateway must know Orders is at `localhost:3001`.
**NATS**:

1.  Gateway publishes message to Subject `create_order`.
2.  NATS Server receives it.
3.  Orders Service is subscribed to `create_order`.
4.  NATS delivers it.

**Benefit**:

- **Location Transparency**: Gateway doesn't care where Orders is.
- **Load Balancing**: Run 5 Order Services. NATS distributes round-robin.
- **Resilience**: Broker can buffer messages.

## 💻 Implementation

### 1. Main.ts

```typescript
const app = await NestFactory.createMicroservice(AppModule, {
  transport: Transport.NATS,
  options: {
    servers: ["nats://localhost:4222"],
  },
});
```

### 2. Client

```typescript
ClientsModule.register([
  {
    name: 'ORDERS_SERVICE',
    transport: Transport.NATS,
    options: { servers: ['nats://localhost:4222'] },
  },
]),
```

## 🧩 Activity / Challenge

1.  Add `nats` service to docker-compose.
2.  Update code to use `Transport.NATS`.
3.  Everything should work exactly as before! (Abstraction power).

## 🔑 Key Takeaways

- **NATS** is extremely lightweight and fast.
- Changing Transport in NestJS is often just a config change.
