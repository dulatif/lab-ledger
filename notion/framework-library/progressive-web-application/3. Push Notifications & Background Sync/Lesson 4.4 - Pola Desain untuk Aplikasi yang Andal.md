# 💡 CT01-4.4: The Reliability Playbook: Offline-First Design Patterns

**Outline:**

- **Beyond the Basics (Presentation):** Introducing the "Outbox" pattern as a robust way to handle multiple offline actions using IndexedDB.
- **The Full Flow (Practice):** A complete, step-by-step code example of saving to IndexedDB, registering a sync, and processing the outbox.
- **Building Your Chat App (Production):** The final project: implementing an offline-first chat message submission.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - Beyond the Basics**

Team, we've mastered the mechanics of Background Sync. But to build a truly reliable application, we need a more robust pattern than just registering a single, named sync. What if the user sends three messages while offline? Registering `'send-new-message'` three times isn't ideal.

The solution is the **Outbox Pattern**. It's a classic offline-first design that works perfectly with Background Sync.

1. When a network request fails, we don't just register a sync. We first serialize the entire request (the data, the URL, the method) into an object and save it to an **IndexedDB** store, which we'll call our "outbox".
2. _Then_, we register a single, generic sync task, like `'process-outbox'`.
3. The `sync` handler in the service worker is now very simple: its only job is to read all the pending requests from the IndexedDB outbox and attempt to send them one by one.

**The Core Philosophy:** IndexedDB provides persistent storage for our failed requests, and Background Sync provides the trigger to process them. This pattern is scalable, robust, and allows us to handle any number of offline actions gracefully. The UI gives optimistic feedback immediately, the data is stored safely, and the service worker cleans up when the network returns.

### **Part 2: Practice - The Full Flow**

Let's walk through the code. This will require a library to make IndexedDB easier, like `idb`. (`npm install idb`)

**Step 1: Create an IndexedDB service** (`db.ts`)

```ts
import { openDB } from 'idb';

const dbPromise = openDB('pwa-db', 1, {
  upgrade(db) {
    db.createObjectStore('outbox', { autoIncrement: true });
  },
});

export async function addToOutbox(requestData) {
  const db = await dbPromise;
  await db.add('outbox', requestData);
}

export async function getAllFromOutbox() {
  const db = await dbPromise;
  return db.getAll('outbox');
}

export async function clearOutbox() {
  const db = await dbPromise;
  await db.clear('outbox');
}
```

**Step 2: Modify the client-side `sendMessage`**

```ts
// In your React component
import { addToOutbox } from './db';

async function sendMessage(message) {
  try {
    // Optimistic attempt
    await fetch('/api/messages', { /* ... */ });
  } catch (error) {
    // If it fails, save to outbox and register sync
    const requestData = { url: '/api/messages', method: 'POST', body: message };
    await addToOutbox(requestData);

    const swRegistration = await navigator.serviceWorker.ready;
    await swRegistration.sync.register('process-outbox');

    // Give optimistic UI feedback
    console.log('Message saved to outbox, will be sent later.');
  }
}
```

**Step 3: Implement the `sync` handler in the Service Worker**

```ts
// In your service worker file (You'll need to import idb here too)
import { getAllFromOutbox, clearOutbox } from './db';

self.addEventListener('sync', (event) => {
  if (event.tag === 'process-outbox') {
    event.waitUntil(processOutbox());
  }
});

async function processOutbox() {
  const pendingRequests = await getAllFromOutbox();
  console.log('Processing outbox with', pendingRequests.length, 'items.');

  for (const req of pendingRequests) {
    try {
      const response = await fetch(req.url, {
        method: req.method,
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(req.body)
      });
      if (!response.ok) throw new Error('Server error');
    } catch (error) {
      console.error('Failed to process request from outbox:', req, error);
      // If one fails, stop and let the browser retry later
      throw error;
    }
  }

  // If all were successful, clear the outbox
  await clearOutbox();
  console.log('Outbox processed and cleared successfully.');
}
```

## 🧠 **Real-World Case Study: Offline Analytics**

This pattern is perfect for non-critical tasks like sending analytics or logging events. If a user is offline, you don't want to lose that valuable data. You also don't want to block the UI. Instead, you can write every analytics event to an "analytics-outbox" in IndexedDB and register a `'process-analytics'` sync. The data will be sent reliably later, without the user even noticing.

## 🤔 **Reflective Questions**

1. In our `processOutbox` function, if the first of five requests fails, we stop and re-throw the error. Why is this a good strategy? What's the alternative?
2. How would you provide feedback to the user that their offline actions have been successfully synced?
3. What are the limitations of this pattern? (e.g., what if the data becomes stale or irrelevant by the time it's synced?)

## 📚 **Further Reading**

- **The `idb` library:** [A small, promise-based wrapper for IndexedDB](https://github.com/jakearchibald/idb)
- **Offline Cookbook:** [A fantastic resource of offline patterns by Jake Archibald](https://web.dev/offline-cookbook/)

## 📝 **Mini Task (Production) - Final Project**

This is it. Let's build the offline-first chat application feature described in the syllabus.

**Task:**

1. Set up IndexedDB in your project using the `idb` library and create an 'outbox' store.
2. Create a simple chat UI with a text input and a "Send" button.
3. Implement the `sendMessage` function on the client. When the "Send" button is clicked:
    - It should immediately add the message to the UI (optimistic update).
    - It should try to `fetch` the server.
    - If the `fetch` fails, it should save the message data to the IndexedDB 'outbox' and register a `'process-outbox'` sync task.
4. Implement the `sync` event handler in your service worker. It should:
    - Listen for the `'process-outbox'` tag.
    - Read all pending messages from IndexedDB.
    - Attempt to `POST` each one to the server.
    - If all sends are successful, clear the outbox.
    - (Bonus) Show a "Your offline messages have been sent" notification upon successful sync.