### **💡 AA02-4.4: Cache Housekeeping: Managing Cache Expiration**

**Outline:**

- **The Metric & The Bottleneck (Presentation):** The problem of "cache bloat" and consuming too much user storage.
- **The Optimization Technique (Practice):** A hands-on guide to using the `workbox-expiration` plugin.
- **Your Performance Mission (Production):** Time to add a responsible expiration policy to your caches.

### 📘 **Full Lesson Content**

### **Part 1: Presentation - The Metric & The Bottleneck**

- **The Goal:** Our goal is to be a good citizen on our users' devices by managing our cache storage responsibly. We need to prevent our caches from growing indefinitely, which could lead to the browser automatically purging them entirely. We are optimizing for **Storage Efficiency** and **Long-Term Reliability**.
- **Identifying the Problem:** Caches have a cost: disk space. Imagine an image-heavy site like a news blog or an e-commerce store. If you cache every single image a user ever sees with a `CacheFirst` or `SWR` strategy, your cache size could quickly grow to hundreds of megabytes or even gigabytes. This is wasteful and can lead to storage pressure on the user's device, especially on mobile. The bottleneck is an unbounded cache.

### **Part 2: Practice - The Optimization Technique**

- **The Principle:** Workbox provides a powerful plugin, `ExpirationPlugin`, that allows you to set limits on your caches. You can configure it to evict entries based on their age or the total number of entries in the cache.
    
- **Secure Code Implementation:** The `ExpirationPlugin` is added to the `plugins` array of any Workbox strategy.
    
    ```ts
    importScripts('<https://storage.googleapis.com/workbox-cdn/releases/6.4.1/workbox-sw.js>');
    const { routing, strategies } = workbox;
    const { ExpirationPlugin } = workbox.expiration;
    
    // SWR strategy for images
    routing.registerRoute(
      ({ request }) => request.destination === 'image',
      new strategies.StaleWhileRevalidate({
        cacheName: 'image-cache',
        plugins: [
          // This is the magic!
          new ExpirationPlugin({
            // Only keep a maximum of 60 images in this cache.
            maxEntries: 60,
    
            // And only keep them for 30 days. After that, they're evicted.
            maxAgeSeconds: 30 * 24 * 60 * 60, // 30 Days
          })
        ]
      })
    );
    
    ```
    
    With this configuration, Workbox will automatically remove the oldest images once the cache contains more than 60 entries, ensuring it never grows too large.
    

### 🧠 **Real-World Case Study: "The E-Commerce Product Grid"**

- **The Scenario:** A user is browsing an e-commerce website, looking at hundreds of different products across many categories.
- **The Problem:** The site uses a `StaleWhileRevalidate` strategy for product images to make navigation feel fast. Without an expiration policy, after an hour of browsing, the user might have 500+ product images stored in their cache, taking up 200MB of space. Most of these are for products they looked at once and will never see again.
- **The Solution:** The developers add the `ExpirationPlugin` to their image caching strategy. They set `maxEntries: 100`. Now, as the user browses past their 100th unique product, Workbox begins to automatically evict the least-recently-used images from the cache. The cache size stays bounded and reasonable, focusing only on the products the user has viewed most recently, while still providing a snappy experience.

### 🤔 **Reflective Questions**

1. What's the difference in behavior between `maxEntries` and `maxAgeSeconds`? Can you use both at the same time? What happens if you do?
2. If you set `maxEntries: 50` on a cache that currently has 70 items, what does Workbox do? Which 20 items get deleted?
3. The `ExpirationPlugin` can also be configured with `purgeOnQuotaError: true`. What browser problem does this setting help you solve automatically?

### 📚 **Further Reading**

- **Workbox Docs:** [`ExpirationPlugin`](https://developer.chrome.com/docs/workbox/modules/workbox-expiration/)
- **web.dev:** [Managing storage quota](https://www.google.com/search?q=https://web.dev/storage-for-the-web/%23managing-quota)
- **MDN:** [Storage API](https://developer.mozilla.org/en-US/docs/Web/API/Storage_API) (Provides context on browser storage limits).

### 📝 **Mini Task (Production)**

- **Your Mission:** You have an existing `StaleWhileRevalidate` strategy for your API calls (`/api/`) that could grow very large over time. Your task is to add a responsible expiration policy.
    1. Import `ExpirationPlugin` from `workbox.expiration`.
    2. Locate the `registerRoute` call for `/api/`.
    3. Inside the `StaleWhileRevalidate` strategy options, add a `plugins` array.
    4. Instantiate a new `ExpirationPlugin` inside the array.
    5. Configure the plugin to only keep a maximum of `50` API responses (`maxEntries`).
    6. Additionally, configure it to automatically delete any API response that is older than 7 days (`maxAgeSeconds`).