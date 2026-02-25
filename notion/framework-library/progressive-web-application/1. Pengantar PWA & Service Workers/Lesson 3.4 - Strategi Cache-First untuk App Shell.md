# 💡 CD03-3.4: The Cache-First Strategy for the App Shell [undone]

**Outline:**

- **The Modern Web App (Presentation):** The moment of truth: serving from the cache.
- **The Engine Room (Practice):** Implementing the `fetch` event handler to intercept requests.
- **Your First Offline Feature (Production):** Time to make your app load with no network.

## 📘 **Full Lesson Content**

### Part 1: Presentation - The Moment of Truth

So far, we have registered a service worker, used the `install` event to precache our app shell, and used the `activate` event to clean up old caches. Now, we'll implement the final, most critical piece: using the cached assets.

We will do this inside the `fetch` event handler. This event fires for every single network request your application makes. Our goal is to implement a **"Cache-First"** strategy for our app shell assets.

The logic is simple and powerful:

1. A request is made (e.g., for `/index.html`).
2. Our service worker's `fetch` event fires, intercepting the request.
3. We first check our cache: "Do I have a saved response for this request?"
4. **If YES:** We immediately serve the response from the cache. The network is never touched. This is extremely fast.
5. **If NO:** The request is not for an app shell asset we've precached. In this case, we let the request proceed to the network as it normally would.

This strategy ensures that our core UI is always served instantly from the local cache, making the app reliable and performant.

### Part 2: Practice - Implementing the `fetch` Event

To implement this, we use the `event.respondWith()` method. This method tells the browser, "Stop! Don't go to the network. I will provide the response myself." We pass it a promise that resolves with a `Response` object.

We'll use `caches.match()` to check our cache. If it returns a valid response, we use it. If it returns `undefined` (a cache miss), we fall back to making a network request with `fetch()`.