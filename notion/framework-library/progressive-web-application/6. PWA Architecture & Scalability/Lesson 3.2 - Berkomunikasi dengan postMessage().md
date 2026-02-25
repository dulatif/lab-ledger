# 💡 AT01.3.2: The Messenger Service: Communicating with postMessage

**Outline:**

- **The Communication Bridge (Presentation):** Introducing the `postMessage()` API as the standard way to send data between the UI and the Service Worker.
- **Sending and Listening (Practice):** A code-driven walkthrough of sending messages in both directions.
- **The Handshake (Production):** Implementing a simple two-way communication flow.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Communication Bridge**

Since the UI and the service worker can't directly access each other's variables, they need a messenger. That messenger is the **`postMessage()` API**. It allows us to send data (serializable objects, strings, numbers) from one context and listen for it in the other, and vice versa.

The flow is always the same:

1. **Sender:** Calls `postMessage()` with a data payload.
2. **Receiver:** Has an event listener for the `'message'` event. The data payload is available in the event object.

This is an asynchronous, event-driven model. You're not calling a function and getting an immediate return value. You're dispatching a message and trusting that the other side is listening and will act on it. This pattern is fundamental to coordinating complex behavior between your app's interface and its background brain.

### **Part 2: Practice - Sending and Listening**

Let's look at the code for both directions.

**Direction 1: UI (Client) → Service Worker** This is useful for telling the service worker to perform an action, like clearing a cache or skipping its waiting phase.

**In your React Component:**

```tsx
function App() {
  const handleClearCache = () => {
    // Check if the service worker is active and ready
    if (navigator.serviceWorker.controller) {
      navigator.serviceWorker.controller.postMessage({
        type: 'CLEAR_IMAGE_CACHE',
        payload: { olderThan: '24h' },
      });
    }
  };

  return <button onClick={handleClearCache}>Clear Image Cache</button>;
}
```

**In your Service Worker (`sw.ts`):**

```ts
self.addEventListener('message', (event) => {
  if (event.data && event.data.type === 'CLEAR_IMAGE_CACHE') {
    // Logic to open the cache and delete old images...
    console.log('Received message from client:', event.data.payload);
  }
});
```

**Direction 2: Service Worker → UI (Client)** This is useful for the service worker to notify the UI of background events, like a successful sync or a new version of the app being available.

**In your Service Worker (`sw.ts`):**

```ts
async function notifyClientsOfUpdate() {
  const clients = await self.clients.matchAll();
  clients.forEach(client => {
    client.postMessage({
      type: 'APP_UPDATE_AVAILABLE',
    });
  });
}
```

**In your React App's root (e.g., in a `useEffect`):**

```ts
useEffect(() => {
  const handleMessage = (event) => {
    if (event.data && event.data.type === 'APP_UPDATE_AVAILABLE') {
      // Logic to show a "New version available" toast notification
      console.log('Received message from service worker!');
    }
  };

  navigator.serviceWorker.addEventListener('message', handleMessage);

  return () => {
    navigator.serviceWorker.removeEventListener('message', handleMessage);
  };
}, []);
```

## 🧠 **Real-World Case Study: "The 'Skip Waiting' Button"**

- **The Goal:** When a new version of the PWA is ready, show the user a button that says "Update available. Refresh." Clicking it should immediately activate the new service worker.
- **The Implementation:**
    1. The UI detects that a new service worker is in the `waiting` state. It displays the button.
    2. When the user clicks the button, the UI calls `navigator.serviceWorker.controller.postMessage({ type: 'SKIP_WAITING' });`.
    3. The _current, active_ service worker has a `message` listener. When it receives the `'SKIP_WAITING'` message, it calls `self.skipWaiting()`.
    4. This promotes the new service worker to `active`, which triggers a `'controllerchange'` event in the UI.
    5. The UI listens for `'controllerchange'` and calls `window.location.reload()` to refresh the page and load the new assets.
- **The Result:** A perfectly seamless, user-controlled update flow, all orchestrated by a single `postMessage` call.

## 🤔 **Reflective Questions**

1. The data sent via `postMessage` must be "serializable." What does this mean, and what types of data can't you send?
2. In the SW → Client example, we use `self.clients.matchAll()`. Why is this necessary? Can't the service worker just know which client sent a message?
3. What are some potential race conditions or error scenarios you need to consider when using `postMessage`? (e.g., what if you send a message before the service worker is active?).

## 📚 **Further Reading**

- **MDN:** [Client.postMessage()](https://developer.mozilla.org/en-US/docs/Web/API/Client/postMessage)
- **MDN:** [ServiceWorker.postMessage()](https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorker/postMessage)
- **Web.dev:** [Broadcast a message to all open tabs from the service worker](https://www.google.com/search?q=https://web.dev/articles/service-worker-communications%23broadcast-a-message-to-all-open-tabs-from-the-service-worker)

## 📝 **Mini Task (Production)**

Let's establish your first two-way communication channel.

1. In a React component, add a button with the text "Get SW Version".
2. When the button is clicked, send a message to the service worker: `{ type: 'GET_VERSION' }`.
3. In your service worker, add a `message` listener. When it receives the `'GET_VERSION'` message, it should define a version string (e.g., `'v1.0.1'`).
4. The service worker must then get a reference to the client that sent the message (`event.source`) and use `event.source.postMessage(...)` to send a reply back: `{ type: 'VERSION_RESPONSE', version: 'v1.0.1' }`.
5. In your React component, set up a `message` listener on `navigator.serviceWorker` to listen for the `'VERSION_RESPONSE'` and `console.log` the version it received.