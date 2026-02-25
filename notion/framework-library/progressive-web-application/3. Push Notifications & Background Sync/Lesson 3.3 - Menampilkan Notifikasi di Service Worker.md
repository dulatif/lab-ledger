# 💡 CT01-3.3: The Arrival: Displaying Notifications in the Service Worker

**Outline:**

- **The `push` Event (Presentation):** Understanding that the service worker is the entry point for incoming push messages from the push service.
- **Listening and Showing (Practice):** The code for adding a `push` event listener and using `self.registration.showNotification()`.
- **Bringing It to Life (Production):** Time to implement the service worker logic to finally see your notification on screen.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The `push` Event**

We've successfully sent a message from our server. The push service has received it and delivered it to the user's device. But who is listening for it?

The browser tab might be closed. The PWA might not be running. The only part of our application that is guaranteed to be woken up by the system to handle this incoming message is our **Service Worker**.

When a push message arrives, the browser fires a `push` event on the global scope of our service worker. Our job is to add an event listener for this event. This is the final and most critical link in the chain.

**The Core Philosophy:** The service worker acts as a background event handler. It receives the data payload from the server and is solely responsible for using the Notifications API to present that data to the user. All display logic lives here.

### **Part 2: Practice - Listening and Showing**

The implementation happens in our service worker file (e.g., `src/sw.ts` if you're using Vite). We add an event listener for `push` just like we would for `fetch` or `install`.

The event object (`event`) has a `data` property, which we can use to get the payload we sent from the server. The payload can be read as text, a blob, an array buffer, or most usefully, as JSON.

Once we have the data, we call `self.registration.showNotification()`. This is the function that actually creates the visible system notification. At a minimum, it takes a title.

```ts
// In your service worker file (e.g., src/sw.ts)

// Make sure TypeScript knows about the Service Worker global scope
declare let self: ServiceWorkerGlobalScope;

self.addEventListener('push', (event) => {
  console.log('Push event received!', event);

  let data = { title: 'New Message', body: 'You have a new message.' };
  if (event.data) {
    try {
      data = event.data.json();
    } catch (e) {
      console.error('Push event data was not valid JSON.', e);
    }
  }

  const title = data.title;
  const options = {
    body: data.body || 'Something new happened!',
    // More options can be added here (icon, badge, etc.)
  };

  // We must wait until the notification is shown
  // before the service worker can go back to sleep.
  event.waitUntil(
    self.registration.showNotification(title, options)
  );
});
```

The `event.waitUntil()` is crucial. It tells the browser not to terminate our service worker until the promise passed to it (in this case, the promise from `showNotification`) has resolved. This ensures our notification has time to display properly.

## 🧠 **Real-World Case Study: Fallback Content**

What if your server sends a push message with no payload, or the payload is corrupted? As seen in the code above, a robust service worker should always have default content. It checks if `event.data` exists and wraps the `json()` parsing in a `try...catch` block. If anything goes wrong, it can still show a generic notification like "New update available" instead of crashing. This ensures a graceful failure and a better user experience.

## 🤔 **Reflective Questions**

1. Why is it important to use `event.waitUntil()`? What could happen if you omitted it?
2. The `showNotification` function is asynchronous and returns a promise. Why do you think that is?
3. If the user has multiple browser profiles on their machine, and your PWA is subscribed in both, what happens when you send a single push notification?

## 📚 **Further Reading**

- **MDN - The `push` event:** [The event that starts it all](https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorkerGlobalScope/push_event)
- **MDN - `showNotification()`:** [The method that displays the notification](https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorkerRegistration/showNotification)

## 📝 **Mini Task (Production)**

Let's finally display our notification.

**Task:**

1. Open your service worker file (`src/sw.ts` or similar).
2. Add the `push` event listener code from the practice section above.
3. Rebuild your PWA to ensure the service worker is updated (`npm run build`).
4. Serve the built files and open your app to register the new service worker.
5. Subscribe to notifications if you aren't already.
6. Trigger your server's `/api/send-notification` endpoint again.
7. This time, you should see a native OS notification appear on your screen with the title "Test Notification". You have completed the full end-to-end push notification flow!