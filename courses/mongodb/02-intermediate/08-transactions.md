# Lesson 8: ACID Transactions

## Learning Goals

- Atomicity.
- Sessions.

## 1. When to use Transactions?

MongoDB is atomic at the **Document Level**.
Use Multi-Document Transactions (ACID) only when you need to update multiple collections/documents in an "All or Nothing" way (e.g., Bank Transfer).

## 2. Usage

Requires a Replica Set (not standalone).

```javascript
const session = client.startSession();
session.startTransaction();

try {
  await coll1.insertOne({ ... }, { session });
  await coll2.updateOne({ ... }, { session });
  await session.commitTransaction();
} catch (error) {
  await session.abortTransaction();
} finally {
  session.endSession();
}
```

## Key Takeaways

- Transactions incur a performance penalty.
- Data modeling (Embedding) often removes the need for transactions.
