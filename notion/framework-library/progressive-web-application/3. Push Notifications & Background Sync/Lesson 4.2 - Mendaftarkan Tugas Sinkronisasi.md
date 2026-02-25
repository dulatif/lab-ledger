# 💡 CT01-4.2: "I'll Do It Later": Registering a Sync Task

**Outline:**

- **Catching the Failure (Presentation):** The core pattern: intercepting a failed network request and registering a sync task as the fallback.
- **`sync.register()` (Practice):** The code for requesting a sync with a specific tag name from the service worker registration.
- **Seeing is Believing (Production):** Time to use Chrome DevTools to see your sync task get registered when you go offline.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - Catching the Failure**

The starting point for using Background Sync is almost always the `catch` block of a failed `fetch` request. Our application attempts to communicate with the server as normal. If it succeeds, great. If it fails due to a network error, instead of giving up, we spring into action.

This is the moment we ask the service worker to take over. We say, "This request failed, but the user's intent was valid. Please register a sync task to complete this when you can."

**The Core Philosophy:** We design for success but plan for failure. The `sync.register()` call is our plan. It's a formal request to the browser to run a piece of our code (in the service worker) at a more opportune time. The "tag" we provide is a crucial piece of information—it tells the service worker _what kind_ of task needs to be completed.

### **Part 2: Practice - `sync.register()`**

Registering a sync is an asynchronous operation. You first need to get the service worker registration, then call the `register` method on its `sync` property.

Let's imagine we have a function to send a chat message.

```ts
async function sendMessage(message) {
  try {
    // We try to send the message as normal
    const response = await fetch('/api/messages', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(message)
    });

    if (!response.ok) {
      // Handle server errors (e.g., 400, 500)
      throw new Error('Server error');
    }

    console.log('Message sent successfully!');

  } catch (error) {
    // This block runs if the fetch fails (e.g., no network)
    console.error('Failed to send message, attempting to sync later.', error);

    // Check for sync support before trying to register
    if ('serviceWorker' in navigator && 'SyncManager' in window) {
      const swRegistration = await navigator.serviceWorker.ready;
      try {
        // Register a sync task with a specific tag
        await swRegistration.sync.register('send-new-message');
        console.log('Sync task "send-new-message" registered.');
        // Here, we would also give the user optimistic UI feedback
      } catch (syncErr) {
        console.error('Could not register sync task', syncErr);
      }
    }
  }
}
```

The tag `'send-new-message'` is a simple, human-readable identifier. When the `sync` event fires in our service worker, this tag will be available, allowing us to route the event to the correct logic.

## 🧠 **Real-World Case Study: Chrome DevTools**

Chrome DevTools provides excellent visibility into Background Sync.

1. Open DevTools and go to the **Application** tab.
2. In the menu on the left, find **Background Sync**.
3. Go to the **Network** tab and check the "Offline" checkbox to simulate being offline.
4. Trigger the action in your PWA (e.g., click the "Send" button that calls `sendMessage`).
5. Switch back to the **Application** tab. You will see an entry in the Background Sync table with your tag name (`send-new-message`) and a "pending" status. This is proof that your registration worked. When you uncheck "Offline", the browser will attempt the sync, and the entry will disappear.

## 🤔 **Reflective Questions**

1. If you register two sync tasks with the exact same tag name while offline, what do you expect to happen when connectivity is restored? Will the `sync` event fire once or twice?
2. The `register()` function returns a promise. When does this promise resolve? Does it wait for the sync to complete?
3. Besides network failure, are there other error conditions where you might want to fall back to registering a sync?

## 📚 **Further Reading**

- **MDN - SyncManager.register():** [The official documentation for the register method](https://developer.mozilla.org/en-US/docs/Web/API/SyncManager/register)
- **Debugging PWAs:** [Google's guide to debugging PWAs, including Background Sync](https://developer.chrome.com/docs/devtools/progressive-web-apps/)

## 📝 **Mini Task (Production)**

Let's register our first sync task.

**Task:**

1. Create a button in your React app labeled "Send Test Message".
2. When clicked, it should call a function similar to the `sendMessage` example. For the `fetch` call, you can just use a placeholder endpoint like `/api/messages`.
3. Implement the `catch` block to register a sync task with the tag `'send-new-message'`.
4. Use Chrome DevTools to test it:
    - Go offline using the Network tab.
    - Click your button.
    - Verify in the Application -> Background Sync tab that the task is pending.
    - Go back online and watch the task disappear.