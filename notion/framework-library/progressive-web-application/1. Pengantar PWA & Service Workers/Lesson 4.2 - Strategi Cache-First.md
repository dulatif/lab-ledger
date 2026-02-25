# 💡 CD04-4.2: The Cache-First Strategy [undone]

**Outline:**

- **The Modern Web App (Presentation):** Defining the go-to strategy for reliability.
- **The Engine Room (Practice):** Implementing `event.respondWith` to serve from the cache.
- **Your First Cached Response (Production):** Verifying that your app is no longer hitting the network for its shell.

## 📘 **Full Lesson Content**

### Part 1: Presentation - The Strategy for Reliability

In the last module, you learned how to precache your app shell. Now, you will learn how to use it. The **Cache-First** strategy is the most common and effective method for making your app shell instantly available and offline-capable.

The logic is exactly as the name implies:

1. **Intercept:** The service worker intercepts a `fetch` event.
2. **Check Cache First:** It first checks the cache to see if a matching response already exists.
3. **Serve or Fetch:**
    - If a match is found, the response is served directly from the cache. The network is completely ignored. This is what makes it so fast and reliable.
    - If no match is found, the request is then passed along to the network.

This strategy is perfect for assets that don't change often, which is the definition of an app shell. For your core `index.html`, CSS, and JavaScript bundles, you want the speed and reliability of serving them from a local cache every single time.

### Part 2: Practice - Implementing `respondWith`

To take control of the response, we use the `event.respondWith()` method. This tells the browser to wait for the promise we provide to resolve with a `Response` object.

We combine this with `caches.match()` to look up the request in our caches.