# 💡 CM01-4.4: Always Fresh: The Network-First Strategy for Critical Data

**Outline:**

- **When Freshness is Key (Presentation):** Explaining the Network-First strategy for data that must be current, with offline as a fallback.
- **The Online Priority (Practice):** Implementing the `NetworkFirst` strategy with Workbox for a specific, critical piece of data.
- **Protecting Your Core Data (Production):** Time to apply this strategy to ensure your most important content is always up-to-date.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - When Freshness is Key**

We have strategies for speed (`StaleWhileRevalidate`) and for efficiency (`CacheFirst`). But what about data that you want to be as up-to-date as possible, every single time? Think about the content of a specific document, a user's account settings, or the items in a shopping cart. For these, showing stale data can be confusing or even dangerous. This is where **Network-First** comes in.

The logic is the inverse of `CacheFirst`:

1. When a request is made, go to the network immediately.
2. If the network request succeeds, return the fresh response and update the cache with the new version.
3. If the network request fails (e.g., the user is offline), _then_ fall back to the cache and return the most recently stored version.

**The Core Philosophy:** This strategy prioritizes data freshness. It ensures that if the user is online, they are always seeing the most current data from the server. The cache is treated purely as a fallback for when the network is unavailable.

### **Part 2: Practice - The Online Priority**

The implementation in Workbox is, by now, familiar. We identify a critical route and apply the `NetworkFirst` strategy.

Let's say we have a news app. The list of headlines can be `StaleWhileRevalidate`, but when a user clicks to read a specific article, we want to try and get the very latest version of that article text.

```ts
// src/sw.ts
import { registerRoute } from 'workbox-routing';
import { NetworkFirst } from 'workbox-strategies'; // 1. Import the strategy

// ... other code ...

// Route for single articles, where freshness is important
registerRoute(
  // Match URLs like /api/articles/12345
  ({ url }) => url.pathname.startsWith('/api/articles/'),
  // Use the NetworkFirst strategy
  new NetworkFirst({
    cacheName: 'article-content-cache',
    plugins: [
      // Optional: Add a timeout. If the network is too slow,
      // fall back to the cache faster.
      new workbox.background_sync.BackgroundSyncPlugin('article-queue', {
        maxRetentionTime: 24 * 60 // Retry for up to 24 hours
      })
    ]
  })
);
```

This code tells Workbox: "When the app asks for a specific article, try the network first. If you get a response, show it and update the `article-content-cache`. If the network fails, serve the last good version from that cache instead."

## 🧠 **Real-World Case Study: Online Banking App**

When you log into your banking PWA and check your account balance, you are seeing a `NetworkFirst` strategy. The app _must_ try the network first to get the real-time, accurate balance. Showing you a stale balance from yesterday could be highly misleading. However, if you're offline, showing you that stale balance from the last time you were connected is infinitely more useful than showing a "Cannot connect to server" error. It provides a sensible, read-only fallback.

## 🤔 **Reflective Questions**

1. What is the primary user experience trade-off when using `NetworkFirst` compared to `StaleWhileRevalidate`?
2. If the user is on a very slow and flaky 3G connection, what might be the downside of `NetworkFirst`? How could a network timeout option help?
3. `NetworkFirst` is often used for HTML pages themselves (the navigation requests). Why does this make sense for a multi-page application that isn't a Single Page App?

## 📚 **Further Reading**

- **Workbox Strategy Docs:** [NetworkFirst](https://www.google.com/search?q=https://developer.chrome.com/docs/workbox/modules/workbox-strategies/%23network-first)
- **Web.dev Pattern:** [Network-First pattern explained](https://www.google.com/search?q=https://web.dev/patterns/caching/network-first/)

## 📝 **Mini Task (Production)**

Let's protect your app's most critical data.

**Task:**

1. Identify a route in your application that fetches a single, specific item where data freshness is important (e.g., a route like `/api/post/:id` or `/api/user/profile`).
2. In your `src/sw.ts`, add a new `registerRoute` rule for this specific route pattern.
3. Apply the `NetworkFirst` strategy to this route, giving it an appropriate `cacheName`.
4. Build and serve your app.
5. **Test it:**
    - Navigate to a detail page to populate the cache while online.
    - In DevTools, set the network to "Offline".
    - Try to reload that same detail page. It should fail to reach the network but successfully fall back to serving the content from your cache. You've made your app's core content resilient.