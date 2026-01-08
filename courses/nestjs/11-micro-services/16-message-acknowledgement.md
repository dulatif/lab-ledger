# 16. Message Acknowledgement

## 🎯 Learning Goal

Ensure no message is lost.

## 🧠 Concept

By default, RabbitMQ might "Auto-Ack" (assume delivered as soon as sent).
If the Consumer crashes _during_ processing, the message is lost forever.

**Manual Ack**:

1.  RabbitMQ sends message.
2.  Consumer processes it (Save to DB, Send Email).
3.  Consumer sends "ACK".
4.  RabbitMQ deletes the message.

If Consumer crashes at step 2, RabbitMQ waits for connection close, then Re-Queues the message to another consumer.

## 💻 Implementation

```typescript
@MessagePattern('alarm_confirmed')
async handle(@Payload() data: unknown, @Ctx() context: RmqContext) {
  const channel = context.getChannelRef();
  const originalMsg = context.getMessage();

  try {
    await this.doWork(data);
    channel.ack(originalMsg);
  } catch (error) {
    // Retry or Dead Letter
    // channel.nack(originalMsg);
  }
}
```

## 🧩 Activity / Challenge

1.  Set `noAck: false` in main.ts options.
2.  Throw an error in your handler.
3.  Restart your service.
4.  See the message instantly redelivered!

## 🔑 Key Takeaways

- **At-Least-Once Delivery**: Manual Ack guarantees the message is processed at least once (might be twice if ack fails).
