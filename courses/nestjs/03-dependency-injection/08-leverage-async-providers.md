# 8. Leverage Async Providers

## 🎯 Learning Goal

Master asynchronous provider initialization to handle tasks like establishing database connections or fetching remote config before your app starts.

## 🧠 Concept

In Node.js, I/O operations (like connecting to a DB) are asynchronous.
If your `DatabaseService` needs to be "connected" before it can be used, you can't just return it synchronously.

NestJS supports **Async Providers**.
If your `useFactory` function returns a **Promise**, NestJS will **wait** for that promise to resolve before instantiating any module that depends on it. ⏳

## 💻 Implementation

### 1. The Async Factory

Just make your factory function `async`.

```typescript
{
  provide: 'ASYNC_CONNECTION',
  useFactory: async () => {
    const connection = await createConnection(); // ⏳ App waits here...
    console.log('Connection established!');
    return connection;
  },
}
```

### 2. Dependency Chain

If `UsersService` injects `'ASYNC_CONNECTION'`, NestJS will:

1.  Start initializing `AppModule`.
2.  See `UsersService` needs `'ASYNC_CONNECTION'`.
3.  Run the factory for `'ASYNC_CONNECTION'`.
4.  **Wait** for the promise.
5.  Pass the resolved connection to `UsersService`.
6.  Finish initializing `AppModule`.

If the promise rejects (throws error), the application boot process fails (which is good—you don't want to start without a DB!).

### 3. Example with Dependencies

Combine it with `inject` for powerful startup logic.

```typescript
{
  provide: 'API_CLIENT',
  useFactory: async (config: ConfigService) => {
    const apiKey = config.getKey();
    const client = new ApiClient(apiKey);
    await client.connect(); // Verify key is valid
    return client;
  },
  inject: [ConfigService],
}
```

## 🧩 Activity / Challenge

1.  Create an async provider called `'DELAYED_DATA'`.
2.  In `useFactory`, use `await new Promise(r => setTimeout(r, 2000))` to simulate a 2-second delay.
3.  Inject the result into your AppController.
4.  Start your app. Notice the console logs—it will take 2 seconds longer than usual to show "Nest application successfully started".

## 🔑 Key Takeaways

- If `useFactory` returns a Promise, NestJS waits for it to resolve.
- This is the standard way to handle asynchronous initialization (DB connections, secrets managers).
- Dependent modules/services are blocked until the async provider is ready.
