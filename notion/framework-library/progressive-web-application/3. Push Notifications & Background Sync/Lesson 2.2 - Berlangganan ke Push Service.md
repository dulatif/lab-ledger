# 💡 CT01-2.2: Getting the Address: Subscribing to the Push Service

**Outline:**

- **Permission is Not Enough (Presentation):** Explaining the next step: getting a unique `PushSubscription` object.
- **The `pushManager.subscribe()` API (Practice):** A deep dive into the subscribe method, its options, and the critical role of the VAPID key.
- **Logging Your First Subscription (Production):** Time to connect the dots and get a real subscription object from the browser's push service.

## 📘 **Full Lesson Content**

**Part 1: Presentation - Permission is Not Enough**

So, the user has clicked "Allow". Great. But all we have is permission. We don't have an "address" to send notifications to. Our next step is to ask the browser's Push Service to generate a unique subscription for this user on this device. This subscription is the magic object that contains the unique endpoint URL we need.

This process must be initiated from our registered Service Worker. The browser gives us access to a `PushManager` interface on the service worker registration object. The key method here is `subscribe()`.

**The Core Philosophy:** Getting permission and getting a subscription are two distinct, asynchronous steps. The permission step is about user consent. The subscription step is a technical handshake between our PWA and the vendor's Push Service, authenticated by our VAPID key. The Service Worker is the intermediary for this handshake.

**Part 2: Practice - The `pushManager.subscribe()` API**

Once we have permission, we can call `subscribe()`. This method takes an `options` object and returns a Promise that resolves with a `PushSubscription` object.

There are two critical options:

- `userVisibleOnly: true`: This is mandatory. It's a promise to the browser that every push notification you send will result in a visible notification to the user. You cannot use web push for silent, background-only tasks.
- `applicationServerKey`: This is your VAPID public key. You must provide it here. The browser needs it to associate this subscription with your server.

Here's how it looks in code, building on our previous permission check:

```ts
async function subscribeUser() {
  // First, get the service worker registration
  const swRegistration = await navigator.serviceWorker.ready;

  // Then, subscribe the user
  try {
    const subscription = await swRegistration.pushManager.subscribe({
      userVisibleOnly: true,
      applicationServerKey: urlBase64ToUint8Array('YOUR_VAPID_PUBLIC_KEY')
    });
    console.log('User is subscribed:', subscription);
    // TODO: Send this subscription to your server
  } catch (error) {
    console.error('Failed to subscribe the user: ', error);
  }
}

// VAPID keys from the server are base64 encoded,
// but the browser needs them as a Uint8Array.
function urlBase64ToUint8Array(base64String) {
  const padding = '='.repeat((4 - base64String.length % 4) % 4);
  const base64 = (base64String + padding)
    .replace(/\\-/g, '+')
    .replace(/_/g, '/');

  const rawData = window.atob(base64);
  const outputArray = new Uint8Array(rawData.length);

  for (let i = 0; i < rawData.length; ++i) {
    outputArray[i] = rawData.charCodeAt(i);
  }
  return outputArray;
}
```

The `PushSubscription` object that gets logged contains the all-important `endpoint` URL and the necessary security keys.

## 🧠 **Real-World Case Study: Multi-Device Sync**

Think about a chat app like Telegram or WhatsApp. You have it on your phone and your desktop. When someone sends you a message, you get a notification on both devices. This is because when you logged in on each device, the app went through this subscription flow. Your user account on their server is associated with two (or more) different `PushSubscription` objects. When your Application Server decides to send a notification, it loops through all the subscriptions associated with your account and sends a push message to each one.

## 🤔 **Reflective Questions**

1. Why do you think the `subscribe` method is on the service worker registration and not on a global object like `window.Notification`?
2. The `PushSubscription` object contains an `endpoint` and `keys`. What do you think the purpose of the `keys` (`p256dh` and `auth`) are? (Hint: It's for payload encryption).
3. What happens if you call `subscribe()` when the user is already subscribed?

## 📚 **Further Reading**

- **MDN - PushManager.subscribe():** [Detailed documentation for the subscribe method](https://developer.mozilla.org/en-US/docs/Web/API/PushManager/subscribe)
- **VAPID Explained:** [A good article on why `applicationServerKey` is necessary](https://www.google.com/search?q=https://blog.ourcade.co/posts/2021/what-is-vapid-for-web-push-notifications/)

## 📝 **Mini Task (Production)**

Let's get a real subscription object.

**Task:**

1. In your project, make sure you have your VAPID public key available (e.g., from a `.env` file as `VITE_VAPID_PUBLIC_KEY`).
2. Add the `urlBase64ToUint8Array` utility function to your code.
3. Modify the logic from the previous lesson. After a user grants permission, call a `subscribeUser` function.
4. Inside `subscribeUser`, get the service worker registration and call `pushManager.subscribe()`, passing the converted VAPID public key.
5. `JSON.stringify` the resulting `PushSubscription` object and log it to the console. Inspect its structure. You have now successfully registered a client with a Push Service!