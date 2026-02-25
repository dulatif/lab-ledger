# 💡 AT01.3.4: The Waiting Room: Creating a Request Queue for Sync

**Outline:**

- **Never Lose Data (Presentation):** The core concept of optimistic UI and queueing failed requests for later synchronization.
- **The Modern Way: Background Sync (Practice):** An introduction to the `BackgroundSync` API for reliable background processing.
- **Queuing Your First Request (Production):** Implementing a basic fetch handler that saves failed requests.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - Never Lose Data**

This is one of the most powerful capabilities of a PWA: the ability to accept user input while offline. If a user fills out a form and clicks "Submit" without a network connection, a standard website would show an error and lose the user's data. A great PWA says, "Got it. I'll send that for you as soon as you're back online."

This is achieved with a **request queue**. The architecture works like this:

1. **Optimistic UI:** When the user clicks "Submit," the UI immediately pretends the submission was successful. It might add the new item to a list in a "pending" state. This provides instant feedback.
2. **The Fetch Attempt:** The app makes the `fetch` request as usual.
3. **The Offline Catch:** The service worker intercepts the request. If it fails due to a network error, instead of letting the error propagate, it _catches_ it.
4. **Queueing:** The service worker saves the essential details of the failed request (URL, method, headers, body) into a persistent storage, usually **IndexedDB**.
5. **Background Sync:** The service worker then tells the browser, "I have some work to do when the network returns."
6. **Replay:** When the browser detects a stable connection has returned, it wakes up the service worker (even if the app's tab is closed!), which then reads the requests from IndexedDB and "replays" them to the server.

This pattern ensures that user data is never lost and provides a seamless, uninterrupted experience.

### **Part 2: Practice - The Modern Way: Background Sync**

While you can build a queueing system manually, modern browsers provide a purpose-built API that makes this much more reliable and efficient: the **Background Sync API**.

**Step 1: The `fetch` handler queues the request** When a `POST` request fails, we save it and register a sync event.

```ts
// sw.ts
import { openDB } from 'idb'; // A simple library for IndexedDB

const dbPromise = openDB('request-queue-db', 1, {
  upgrade(db) {
    db.createObjectStore('requests', { autoIncrement: true });
  },
});

self.addEventListener('fetch', (event) => {
  // Only intercept POST requests to our API
  if (event.request.method !== 'POST' || !event.request.url.includes('/api/')) {
    return;
  }

  event.respondWith(
    fetch(event.request.clone()).catch(async (error) => {
      // Fetch failed. Queue the request.
      const db = await dbPromise;
      const requestData = {
        url: event.request.url,
        method: event.request.method,
        body: await event.request.json(),
      };
      await db.put('requests', requestData);

      // Register the sync event
      await self.registration.sync.register('sync-requests');

      // Return a positive response to the app for an optimistic UI
      return new Response(JSON.stringify({ status: 'queued' }), { status: 202 });
    })
  );
});
```

**Step 2: The `sync` handler replays the queue** This event is fired by the browser when the network is back.

```ts
// sw.ts (continued)
self.addEventListener('sync', (event) => {
  if (event.tag === 'sync-requests') {
    event.waitUntil(replayRequests());
  }
});

async function replayRequests() {
  const db = await dbPromise;
  const requests = await db.getAll('requests');

  for (const request of requests) {
    // Replay the fetch
    await fetch(request.url, {
      method: request.method,
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(request.body),
    });
  }

  // Clear the queue
  await db.clear('requests');
}
```

## 🧠 **Real-World Case Study: "The Offline Messenger"**

- **The App:** A chat PWA that needed to work in areas with intermittent connectivity.
- **The Old Way:** When a user sent a message offline, it would show a "Failed to send" error, and the user would have to manually retry.
- **The New Architecture:** They implemented a request queue using the Background Sync API. When a user sent a message offline, the UI would optimistically add it to the chat history with a small "pending" clock icon next to it. The failed `fetch` was caught by the service worker and stored in IndexedDB.
- **The Magic:** The user could then close the PWA entirely. Later, when they walked into an area with Wi-Fi, the browser would wake up their service worker in the background. The `sync` event would fire, replaying the queued chat messages. The next time the user opened the app, their messages would be there, fully sent, with the "pending" icon gone. The experience was indistinguishable from a native messaging app.

## 🤔 **Reflective Questions**

1. What is the purpose of `event.waitUntil()` inside the `sync` event listener?
2. The Background Sync API is a "progressive enhancement." What does this mean, and what would be a good fallback strategy for browsers that don't support it?
3. Why is it important to store the queued requests in IndexedDB instead of a simple variable in the service worker?

## 📚 **Further Reading**

- **Web.dev:** [Introducing Background Sync](https://www.google.com/search?q=https://web.dev/articles/background-sync)
- **MDN:** [SyncManager API](https://developer.mozilla.org/en-US/docs/Web/API/SyncManager)
- **Jake Archibald:** [Offline Cookbook - Background Sync](https://www.google.com/search?q=https://jakearchibald.com/2014/offline-cookbook/%23background-sync)

## 📝 **Mini Task (Production)**

Let's implement the first half of the queueing mechanism.

1. Add the `idb` library to your project (`npm install idb`).
2. In your service worker, set up the IndexedDB database promise as shown in the example.
3. Modify your `fetch` event listener. Add a condition to only intercept `POST` requests to a specific path (e.g., `/api/submit-form`).
4. Inside the `.catch()` block of the fetch, add the logic to serialize the request (`url`, `method`, `body`) and save it to your 'requests' object store in IndexedDB.
5. Use Chrome DevTools (Application > IndexedDB) to verify that when you are offline and trigger the `POST` request, a new entry appears in your database.