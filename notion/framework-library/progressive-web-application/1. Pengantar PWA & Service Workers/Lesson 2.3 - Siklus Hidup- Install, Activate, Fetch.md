# 💡 CD02-2.3: The Service Worker Lifecycle: Install, Activate, Fetch

**Outline:**

- **The Modern Web App (Presentation):** Understanding the distinct phases of a service worker's life.
- **The Engine Room (Practice):** Listening for lifecycle events in your service worker script.
- **Your First Lifecycle (Production):** Time to see the events fire in the browser console.

## 📘 **Full Lesson Content**

### Part 1: Presentation - A Controlled Life

A service worker's life is very structured. It moves through a predictable lifecycle of events. Understanding this lifecycle is critical because it dictates _when_ and _how_ you should implement features like caching.

The main events are:

1. **`install`**: This is the first event a service worker gets, and it only happens once per service worker version. The browser has downloaded the script and is executing it for the first time. This is your golden opportunity to prepare things. The most common task here is **precaching**—downloading and storing all the static assets that make up your "app shell" (the core HTML, CSS, and JavaScript).
2. **`activate`**: This event fires after the service worker has been successfully installed and is ready to take control of the page. An important detail: if an _old_ service worker is currently controlling the page, the new one will enter a `waiting` state until all tabs controlled by the old worker are closed. The `activate` event is the ideal time to do any cleanup from the previous service worker, such as deleting old, unused caches.
3. **`fetch`**: This is the main event for day-to-day operations. It fires **every single time** your application makes a network request—whether for a page, a script, an image, or an API call. Inside the `fetch` event handler, you decide how to respond to the request: serve it from the cache, let it go to the network, or even construct a new response from scratch.

### Part 2: Practice - Listening to Events

Let's see what this looks like in code. We'll create a very simple service worker file (`sw.ts` or `sw.js`) that just logs a message for each event.

```
// Add this to your service worker file (e.g., public/sw.ts)
// `self` refers to the service worker's global scope

self.addEventListener('install', (event) => {
  console.log('[Service Worker] V1 Installing...');
  // The waitUntil() method ensures that the service worker will not
  // install until the code inside has successfully completed.
  // We will use this for caching later.
  // event.waitUntil(...);

});

self.addEventListener('activate', (event) => {
  console.log('[Service Worker] V1 Activating...');
  // This is a good time to clean up old caches.
});

self.addEventListener('fetch', (event) => {

  // This log will happen for every network request.

  console.log(`[Service Worker] Fetching: ${event.request.url}`);

  // In the next lesson, we will respond to the request here.

  // event.respondWith(...);

});

```

When the browser registers this service worker for the first time, you will see the "Installing..." and "Activating..." messages in the console. Then, as you navigate or as the page fetches assets, you will see a stream of "Fetching..." messages.

## 🧠 **Real-World Case Study: "The Seamless Update"**

Imagine you deploy a new version of your PWA with an updated UI. A user who has the old version open in a tab visits your site again.

1. The browser detects the new service worker file and downloads it in the background.
2. The `install` event fires in the new service worker. It caches all the new CSS and JS assets for the updated UI.
3. The new service worker now enters the `waiting` state. It doesn't take control yet because the old worker is still active. This is a safety mechanism to prevent a situation where the old page (with old HTML) tries to request new assets that might be incompatible.
4. When the user closes all tabs of your app and re-opens it, the old worker is gone.
5. The `activate` event fires in the new worker. It takes control and can now safely clean up the caches from the old version.
6. The user now sees the new, updated UI, and all requests are handled by the new service worker. This provides a smooth, reliable update path.

## 🤔 **Reflection Questions**

1. Why is it important that a new service worker waits to activate until all old tabs are closed? What could go wrong if it activated immediately?
2. The `install` event only happens once. What happens if the caching process fails halfway through (e.g., the user loses their connection)?
3. What is the difference between the `fetch` event in a service worker and a standard JavaScript `fetch()` call in your React component?

## 📚 **Further Reading**

- **Web.dev:**

TheServiceWorkerLifecycle

([https://web.dev/articles/service-worker-lifecycle](https://web.dev/articles/service-worker-lifecycle))

## 📝 **Mini-Task (Production)**

Your task is to witness the service worker lifecycle firsthand.

1. Use the project from the previous lesson where you registered your service worker.
2. Replace the content of your empty `sw.ts` file with the "Lifecycle Logger" code from the **Practice** section above.
3. Open your app in the browser. Open Chrome DevTools and look at both the **Console** and the **Application > Service Workers** tab.
4. Hard refresh the page (Ctrl+Shift+R or Cmd+Shift+R).
5. Observe the console logs. You should see "Installing...", "Activating...", and then a series of "Fetching..." logs for all the assets your page requests. This confirms your service worker is alive and responding to events.