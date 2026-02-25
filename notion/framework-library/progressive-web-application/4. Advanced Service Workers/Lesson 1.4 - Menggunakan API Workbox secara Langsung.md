# 💡 AD01.4: Direct Control: Using the Workbox API

**Outline:**

- **Beyond Precaching (Presentation):** Introducing runtime caching with Workbox routing.
- **Writing Your First Route (Practice):** Implementing a `CacheFirst` strategy for Google Fonts.
- **Caching Your API (Production):** Applying a `StaleWhileRevalidate` strategy to dynamic content.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - Beyond Precaching**

Precaching is powerful, but it's only for your core app shell—the files you know about at build time. What about everything else?

- Fonts loaded from Google Fonts or Adobe Fonts.
- Images uploaded by users and served from a CDN.
- Data fetched from your backend API.

These are all _runtime_ requests. We can't precache them, but we absolutely need to cache them to create a good offline experience. This is where Workbox's routing module comes in.

The concept is simple: you tell Workbox, "When you see a request that matches _this pattern_, handle it using _this caching strategy_." This allows you to create a sophisticated, multi-layered caching system tailored precisely to your application's needs. You can treat fonts differently from API calls, and API calls differently from user avatars. This is the path to true network resilience.

### **Part 2: Practice - Writing Your First Route**

Let's implement a very common pattern: caching Google Fonts. We want to fetch them from the network the first time, but on subsequent visits, we want to load them instantly from the cache to avoid that flash of unstyled text. This is a perfect use case for the `CacheFirst` strategy.

We'll add this to our `src/sw.ts` file.

```ts
// src/sw.ts

/// <reference lib="webworker" />
declare const self: ServiceWorkerGlobalScope;

import { cleanupOutdatedCaches, precacheAndRoute } from 'workbox-precaching';
import { registerRoute } from 'workbox-routing'; // 1. Import the router
import { CacheFirst } from 'workbox-strategies'; // 2. Import the strategy

// --- Precaching logic from previous lesson ---
precacheAndRoute(self.__WB_MANIFEST);
cleanupOutdatedCaches();
// ---------------------------------------------

// 3. Register a new route
registerRoute(
  // The matching function
  ({ url }) => url.origin === '<https://fonts.googleapis.com>',
  // The caching strategy
  new CacheFirst({
    cacheName: 'google-fonts-webfonts',
  })
);

self.addEventListener('fetch', (event) => {
  // The router will now automatically handle matching requests.
  // We don't need to add custom logic here for this to work.
});
```

**Breaking it down:**

- `import { registerRoute } from 'workbox-routing'`: We import the function that allows us to define new rules.
- `import { CacheFirst } from 'workbox-strategies'`: We import the specific strategy class we want to use. Other options include `NetworkFirst`, `StaleWhileRevalidate`, and `NetworkOnly`.
- `registerRoute(matcher, handler)`: This is the core API.
    - The first argument is a _matcher function_. It receives information about the request (like the `url`) and must return `true` if the route should handle this request.
    - The second argument is the _handler_, which is an instance of a caching strategy. We create `new CacheFirst()` and give the cache a descriptive name.

## 🧠 **Real-World Case Study: "The Fast-Loading Blog"**

- **The Problem:** A popular tech blog is fast on desktop but feels slow on mobile with a 3G connection. The reason? It uses a custom web font. On every single navigation, the browser has to re-fetch the font files, delaying the rendering of content.
- **The Solution (`CacheFirst`):** The developers implement the exact `registerRoute` pattern from our practice example. The very first time a user visits, the fonts are downloaded and cached by the service worker. For every subsequent page view or revisit, the fonts are served from the cache in milliseconds. The "flash of unstyled text" is eliminated, and the perceived performance of the site improves dramatically. This small code change directly enhances the user's reading experience.

## 🤔 **Reflective Questions**

1. What's the key difference between a request that is _pre-cached_ and a request that is _runtime-cached_?
2. In the `registerRoute` matcher `({ url }) => url.origin === '<https://fonts.googleapis.com>'`, what would happen if the font was served from `https://fonts.gstatic.com` (which Google Fonts also uses)? How would you fix the matcher?
3. Why is `CacheFirst` a good strategy for versioned assets like fonts, but potentially a bad strategy for something dynamic like `/api/messages`?

## 📚 **Further Reading**

- **Tooling:** [Workbox Routing Module Documentation](https://developer.chrome.com/docs/workbox/modules/workbox-routing)
- **Strategies:** [Workbox Caching Strategies Explained](https://developer.chrome.com/docs/workbox/modules/workbox-strategies)
- **Web.dev:** [Offline Cookbook](https://web.dev/articles/offline-cookbook) (Tons of practical examples and patterns).

## 📝 **Mini Task (Production)**

Time to cache your app's dynamic data.

1. Identify a key API endpoint in your application (e.g., `/api/users`, `/api/products`).
2. In your `src/sw.ts`, import the `StaleWhileRevalidate` strategy from `workbox-strategies`.
3. `registerRoute` for your API endpoint. You can match it with a regular expression, for example: `new RegExp('/api/')`.
4. Use the `StaleWhileRevalidate` strategy for the handler. Give it a descriptive cache name like `'api-cache'`.
5. Build and preview your app. Use the DevTools Network tab to watch the requests. On the first load, it should go to the network. On a refresh, you should see the request being served from the "Service Worker", and a second, separate request being made in the background to update the cache. You've just made your app's data available offline while also keeping it fresh.