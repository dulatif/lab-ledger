# 💡 CT01-4.3: Waking Up: Handling the `sync` Event in the Service Worker

**Outline:**

- **The Other Half (Presentation):** Understanding how the `sync` event in the service worker completes the Background Sync flow.
- **Listening for `sync` (Practice):** The code for adding the `sync` event listener, checking the tag, and replaying the failed request.
- **Completing the Loop (Production):** Time to implement the service worker logic and see your offline action succeed when the network returns.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Other Half**

We've successfully registered a sync task from our client-side code. The browser has noted our request. Now, we need to implement the code that will actually _run_ when the browser decides it's time to perform the sync. This code lives exclusively in our **Service Worker**.

When the browser detects a stable network connection, it checks for any pending sync registrations. For each one, it wakes up the corresponding service worker and fires a `sync` event.

**The Core Philosophy:** The `sync` event handler is our second chance to run the critical logic that failed before. It's a background process, so it must be self-contained and cannot rely on any UI or page state. Its only input is the `tag` that was registered, which it uses to determine exactly what work needs to be done.

### **Part 2: Practice - Listening for `sync`**

The implementation is very similar to handling the `push` event. We add an event listener to the service worker's global scope.

The `event` object for a `sync` event has a crucial property: `event.tag`. This contains the string we used when we called `sync.register()`. We use this tag in a `switch` statement or `if/else` block to route to the correct logic.

Just like with `push`, we must use `event.waitUntil()` to tell the browser to keep the service worker alive until our asynchronous work (like a `fetch` call) is complete.

```ts
// In your service worker file (e.g., src/sw.ts)

declare let self: ServiceWorkerGlobalScope;

self.addEventListener('sync', (event) => {
  console.log('Sync event fired for tag:', event.tag);

  if (event.tag === 'send-new-message') {
    // The specific logic for this tag
    const syncLogic = async () => {
      console.log('Re-sending new message...');
      try {
        // This is where we would get pending data from IndexedDB
        // For this example, we'll just send a hardcoded message
        const response = await fetch('/api/messages', {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify({ text: 'This message was sent via background sync.' })
        });

        if (!response.ok) {
          throw new Error('Server error on sync.');
        }

        console.log('Message re-sent successfully!');
        // Here we could show a notification to the user
        self.registration.showNotification('Message Sent!', {
          body: 'Your offline message has now been delivered.'
        });

      } catch (error) {
        console.error('Failed to re-send message during sync.', error);
        // The browser will automatically try again later with backoff
        throw error; // Re-throwing the error signals the sync failed
      }
    };

    event.waitUntil(syncLogic());
  }
});
```

## 🧠 **Real-World Case Study: The Data Problem**

A key challenge here is: how does the service worker know _what data_ to send? In our simple example, we're sending a hardcoded message, but that's not realistic. The service worker has no access to the state of your React components.

The answer is **IndexedDB**. When the initial `fetch` fails in the client, before registering the sync, you must save the data that needs to be sent (the message content, the form data, etc.) to an IndexedDB database. Then, in the `sync` handler, the service worker's first step is to read that pending data from IndexedDB before making the `fetch` call. This "outbox" pattern is the key to making Background Sync truly robust. We will cover this in the next lesson.

## 🤔 **Reflective Questions**

1. What happens if the `fetch` call inside the `sync` handler also fails?
2. Why is showing a success notification from the `sync` handler a good user experience?
3. If a `sync` event is taking a long time (e.g., uploading a large file), does this block other service worker events like `fetch`?

## 📚 **Further Reading**

- **MDN - The `sync` event:** [Official documentation for the sync event](https://developer.mozilla.org/en-US/docs/Web/API/SyncEvent)
- **Web.dev - Background Sync:** [The section covering the service worker side](https://www.google.com/search?q=https://web.dev/background-sync/%23the-service-worker-side)

## 📝 **Mini Task (Production)**

Let's complete the sync loop.

**Task:**

1. Add the `sync` event listener to your service worker file, as shown in the example.
2. The handler should listen for the `'send-new-message'` tag.
3. When it fires, it should just log a message like "Sync event for send-new-message handled!" to the console. You don't need to make a real `fetch` call yet.
4. Rebuild and register your new service worker.
5. Test the full flow:
    - Go offline.
    - Click the button in your UI to register the sync.
    - Go online.
    - Check your service worker's console (in Application -> Service Workers -> click "inspect") to see your log message. You have now successfully handled a sync event.