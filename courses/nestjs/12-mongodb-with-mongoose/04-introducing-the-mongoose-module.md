# 4. Introducing the Mongoose Module

## 🎯 Learning Goal

Connect NestJS to MongoDB.

## 🧠 Concept

NestJS provides a wrapper around the popular `mongoose` library.

## 💻 Implementation

### 1. Install

```bash
npm install @nestjs/mongoose mongoose
```

### 2. Configure AppModule

```typescript
import { MongooseModule } from "@nestjs/mongoose";

@Module({
  imports: [
    MongooseModule.forRoot("mongodb://localhost:27017/nest-course"),
    CoffeesModule,
  ],
})
export class AppModule {}
```

## 🧩 Activity / Challenge

1.  Run the app (`npm run start:dev`).
2.  Check logs. If it says "MongooseModule dependencies initialized", you are connected.
3.  Stop the docker container. NestJS should crash/retry connection.

## 🔑 Key Takeaways

- `forRoot` establishes the connection pool.
