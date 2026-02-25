# 💡 CD04-4.3: Handling Requests Not in Cache [undone]

**Outline:**

- **The Modern Web App (Presentation):** The importance of the network fallback.
- **The Engine Room (Practice):** Caching dynamic content on the fly.
- **Your First Dynamic Cache (Production):** Caching an API response.

## 📘 **Full Lesson Content**

### Part 1: Presentation - The Importance of the Fallback

In the last lesson, our Cache-First strategy had a crucial component: `|| fetch(event.request)`. This is the **network fallback**. Without it, your PWA would be severely broken.

Any request for a resource that you did not precache—like an API call, an image uploaded by a user, or a font loaded from Google Fonts—would simply fail. The service worker would look in the cache, find nothing, and have no further instructions. The browser would receive no response, and the request would error out.

The network fallback is your escape hatch. It says, "If this request isn't something I'm explicitly managing in the cache, then just behave like a normal web page and go get it from the network." This ensures your app continues to function for all dynamic content and third-party resources while still providing the core app shell from the cache.

### Part 2: Practice - Caching Dynamic Content

We can make our fallback even smarter. What if we want to cache network responses as we receive them? For example, if a user visits `/api/products`, we can fetch it from the network the first time, but also save a copy to the cache. The next time they request it (even offline), we can serve the cached version.

This is often called a "Stale-While-Revalidate" or dynamic caching strategy. Here's a common pattern for caching new requests: