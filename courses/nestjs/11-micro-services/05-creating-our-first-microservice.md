# 5. Creating our first Microservice

## 🎯 Learning Goal

Convert a hybrid app to a pure microservice receiver using TCP.

## 🧠 Concept

The default `nest new` creates an HTTP server (Express/Fastify).
To make it a microservice helper, we change `main.ts`.

## 💻 Implementation

### 1. Configure main.ts

```typescript
// apps/orders/src/main.ts
async function bootstrap() {
  const app = await NestFactory.createMicroservice<MicroserviceOptions>(
    OrdersModule,
    {
      transport: Transport.TCP,
      options: {
        port: 3001,
      },
    }
  );
  await app.listen();
}
bootstrap();
```

### 2. The Controller

Microservices don't handle `@Get()`. They handle `@MessagePattern()`.

```typescript
@Controller()
export class OrdersController {
  @MessagePattern({ cmd: "create_order" })
  create(data: any) {
    return { id: 1, ...data };
  }
}
```

### 3. The Client (Gateway)

The main HTTP app calls the microservice.

```typescript
@Injectable()
export class AppService {
  private client: ClientProxy;

  constructor() {
    this.client = ClientProxyFactory.create({
      transport: Transport.TCP,
      options: { port: 3001 },
    });
  }

  createOrder() {
    return this.client.send({ cmd: "create_order" }, { product: "Phone" });
  }
}
```

## 🧩 Activity / Challenge

1.  Setup the default app as Gateway (HTTP).
2.  Setup `orders` app as Microservice (TCP).
3.  Call `Gateway -> Orders`.

## 🔑 Key Takeaways

- **TCP**: Fast, but point-to-point. You need to know the IP/Port.
- **MessagePattern**: Matches the JSON object sent by the client.
