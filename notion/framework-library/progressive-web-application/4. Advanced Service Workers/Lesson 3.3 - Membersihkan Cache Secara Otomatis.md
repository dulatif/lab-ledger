# 💡 AD01.3.3: The Janitor: Cleaning Caches Automatically

**Outline:**

- **The Problem of Hoarding (Presentation):** Why an unmanaged cache is a ticking time bomb.
- **The Tools of the Trade (Practice):** Implementing `maxEntries` and `maxAgeSeconds` with the `ExpirationPlugin`.
- **Taming the Image Cache (Production):** Applying expiration rules to a real-world scenario.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Problem of Hoarding**

Caches are fantastic for performance and offline capability, but they have a dark side: they can grow forever. If your app caches every article a user reads or every product image they view, after a few months that user could have gigabytes of your app's data stored on their device.

This leads to several problems:

- **Wasted Storage:** It's inconsiderate to fill up a user's device with old, irrelevant data.
- **Stale Content:** Data that was cached months ago is likely out of date. Serving it can be misleading.
- **Browser Eviction:** If your app's storage usage gets too high, the browser may decide to clear it all out without warning, leaving the user with no offline data at all.

A professional-grade PWA must be a good citizen on the user's device. It needs a janitor—an automated process that cleans up old and unneeded cache entries. This ensures the cache stays relevant, fast, and trim.

### **Part 2: Practice - The Tools of the Trade**

Workbox's `ExpirationPlugin` is our janitor. It's an incredibly powerful and easy-to-use plugin that gives us two primary tools for cleaning our caches:

1. `maxEntries`: Limits the number of entries (files/responses) in a cache. Once the limit is exceeded, the _least recently used_ entry is automatically removed.
2. `maxAgeSeconds`: Sets a time limit on entries. Any entry older than the specified duration is considered stale and will be removed the next time the cache is accessed.

Let's see how to apply them.

```ts
// src/sw.ts
// ... (imports)
import { registerRoute } from 'workbox-routing';
import { StaleWhileRevalidate } from 'workbox-strategies';
import { ExpirationPlugin } from 'workbox-expiration'; // 1. Import the plugin

// ... (precaching logic)

// Caching news articles from an API
registerRoute(
  new RegExp('^/api/articles/'),
  new StaleWhileRevalidate({
    cacheName: 'articles-cache',
    plugins: [
      // 2. Add and configure the ExpirationPlugin
      new ExpirationPlugin({
        // Only keep the 50 most recent articles
        maxEntries: 50,
        // And don't keep any article for more than 7 days
        maxAgeSeconds: 7 * 24 * 60 * 60,
      }),
    ],
  })
);
```

With this configuration, our `articles-cache` will never contain more than 50 entries. If a user reads a 51st article, the oldest one they read will be evicted. Furthermore, if they don't visit the site for over a week, the next time they do, any articles older than 7 days will be purged. It's a self-maintaining system.

## 🧠 **Real-World Case Study: "The Social Media Timeline"**

- **Before (No Expiration):** A user scrolls through their social media feed every day. The app uses `StaleWhileRevalidate` to cache API responses for each day's timeline. After a year, the user's cache contains 365 timeline responses, most of which are for posts they will never look at again. The cache is bloated with irrelevant data.
- **After (With Expiration):** The developers add the `ExpirationPlugin` with `maxAgeSeconds: 2 * 24 * 60 * 60` (2 days). Now, the cache only ever holds the timeline data for today and yesterday. This is perfect. It provides a great offline experience for recent content, but doesn't waste space on posts from last Tuesday. The app is faster, lighter, and a better citizen on the user's device.

## 🤔 **Reflective Questions**

1. If you set both `maxEntries` and `maxAgeSeconds`, which rule "wins"? (Hint: The plugin will evict an entry if _either_ condition is met).
2. When would you choose to use `maxEntries` as your primary rule, versus `maxAgeSeconds`?
3. The cleanup process runs when the service worker starts up or when a new entry is added. What does this mean for entries that expire while the user is not using your app?

## 📚 **Further Reading**

- **Workbox Docs:** [`ExpirationPlugin`](https://www.google.com/search?q=https://developer.chrome.com/docs/workbox/reference/workbox-expiration/classes/ExpirationPlugin)
- **Web.dev:** [Cache Storage: The Ultimate Guide](https://www.google.com/search?q=https://web.dev/articles/cache-storage-ultimate-guide) (Explains how browsers manage storage quotas).
- **Workbox Recipes:** [A great example of using expiration for images](https://www.google.com/search?q=https://developer.chrome.com/docs/workbox/workbox-recipes%23image-cache)

## 📝 **Mini Task (Production)**

You have a PWA for a photo gallery. You are caching user-uploaded images from a CDN using a `CacheFirst` strategy. To ensure a good user experience and manage storage, you need to add an expiration policy.

1. Locate or create the route in `src/sw.ts` responsible for caching images.
2. Import and add the `ExpirationPlugin` to this route's strategy.
3. Configure the plugin to meet the following requirements:
    - The cache should hold a maximum of **60** images.
    - Images should be automatically removed from the cache after **30 days**.
4. Add a code comment explaining why this combination of `maxEntries` and `maxAgeSeconds` is a good policy for a photo gallery app.