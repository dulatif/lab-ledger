# 💡 CM01-4.3: Set in Stone: The Cache-First Strategy for Assets

**Outline:**

- **Cache and Forget (Presentation):** Explaining the Cache-First strategy for assets that rarely or never change.
- **The Asset Vault (Practice):** Implementing `CacheFirst` with Workbox, including plugins for expiration.
- **Caching Your Visuals (Production):** Time to apply this strategy to your app's images and fonts.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - Cache and Forget**

While `StaleWhileRevalidate` is great for data, some assets are different. Think about third-party fonts, your company logo, or a user's avatar. These files are not expected to change often, if ever. For these, constantly going to the network to "revalidate" is wasteful. We want a different approach: **Cache-First**.

The logic is simple and direct:

1. When a request is made, look in the cache for a matching response.
2. If one is found, return it immediately. The network is never touched.
3. If nothing is found in the cache, go to the network, get the response, and put a copy in the cache for all future requests.

**The Core Philosophy:** This strategy prioritizes network efficiency. Once an asset is cached, we avoid making redundant network requests for it. It's the perfect choice for static or "versioned" assets where the URL itself would change if the content did (e.g., `styles-v2.css`).

### **Part 2: Practice - The Asset Vault**

Workbox makes this easy. We use `registerRoute` to match the assets we want to cache and apply the `CacheFirst` strategy. It's also a best practice to include "plugins" to manage this cache, like setting an expiration date to prevent it from growing indefinitely.

```ts
// src/sw.ts
import { registerRoute } from 'workbox-routing';
import { CacheFirst } from 'workbox-strategies';
import { ExpirationPlugin } from 'workbox-expiration'; // 1. Import the plugin

// ... other code ...

// Route for caching images
registerRoute(
  // Match any request that is for an 'image'
  ({ request }) => request.destination === 'image',
  // Use the CacheFirst strategy
  new CacheFirst({
    cacheName: 'image-cache',
    plugins: [
      // 2. Add a plugin to manage the cache
      new ExpirationPlugin({
        maxEntries: 60, // Don't cache more than 60 images
        maxAgeSeconds: 30 * 24 * 60 * 60, // Cache them for 30 Days
      }),
    ],
  })
);
```

This code tells Workbox: "When the app requests an image, look in the `image-cache` first. If it's not there, get it from the network and store it. By the way, don't let this cache grow beyond 60 images, and automatically delete any image that's older than 30 days."

## 🧠 **Real-World Case Study: Google Fonts**

Many websites use Google Fonts. The first time you visit a site, your browser requests a specific font file (e.g., `Roboto-Regular.woff2`) from `fonts.gstatic.com`. If the site has a PWA with a `CacheFirst` strategy for fonts, this file is saved to the cache. Every other page you visit on that site, and every time you return, that font is loaded instantly from your local cache. The browser doesn't even attempt to re-download it, making page rendering much faster.

## 🤔 **Reflective Questions**

1. Why is `CacheFirst` a poor strategy for your main `/api/news` endpoint?
2. The `ExpirationPlugin` has `maxEntries` and `maxAgeSeconds`. What is the difference, and why might you use one over the other, or both?
3. If you update a user's avatar image but keep the same URL, what will happen with a `CacheFirst` strategy? How could you solve this problem? (Hint: this is called cache-busting).

## 📚 **Further Reading**

- **Workbox Strategy Docs:** [CacheFirst](https://www.google.com/search?q=https://developer.chrome.com/docs/workbox/modules/workbox-strategies/%23cache-first)
- **Workbox Plugins:** [Using Workbox Plugins](https://developer.chrome.com/docs/workbox/using-plugins/)

## 📝 **Mini Task (Production)**

Let's stop re-downloading the same images over and over.

**Task:**

1. In your `src/sw.ts` file, add a new `registerRoute` rule.
2. The matcher should target image requests. The easiest way is `({ request }) => request.destination === 'image'`.
3. The handler should be a new `CacheFirst` strategy.
4. Give it a `cacheName` like `'static-asset-cache'`.
5. Add the `ExpirationPlugin` to your strategy, configuring it to cache images for a reasonable amount of time (e.g., 7 or 30 days).
6. Build and serve your app. Navigate around a bit to load some images.
7. Go to DevTools -> `Application` -> `Cache Storage`. You should now see your new cache (`static-asset-cache`) populated with the image files your app requested.