# 8. Use Transactions

## 🎯 Learning Goal

Perform multi-document ACID transactions in MongoDB.

## 🧠 Concept

Historical Note: MongoDB didn't always have transactions. It added them in v4.0.
**Requirement**: You MUST be running a **Replica Set**.
A standalone `docker run mongo` DOES NOT support transactions by default in some versions, or requires specific config.

## 💻 Implementation

```typescript
import { Connection } from 'mongoose';
import { InjectConnection } from '@nestjs/mongoose';

constructor(@InjectConnection() private connection: Connection) {}

async createWithTransaction() {
  const session = await this.connection.startSession();
  session.startTransaction();

  try {
    const coffee = new this.coffeeModel(dto);
    await coffee.save({ session }); // Pass session!

    const event = new this.eventModel(eventDto);
    await event.save({ session });

    await session.commitTransaction();
  } catch (error) {
    await session.abortTransaction();
    throw error;
  } finally {
    session.endSession();
  }
}
```

## 🧩 Activity / Challenge

1.  This might fail locally if your Docker container isn't a Replica Set.
2.  If it fails, treat this as a "Theory" lesson or look up "converting mongo docker to replicaset".

## 🔑 Key Takeaways

- Pass `{ session }` to EVERY operation in the transaction. If you miss one, it executes outside the transaction.
