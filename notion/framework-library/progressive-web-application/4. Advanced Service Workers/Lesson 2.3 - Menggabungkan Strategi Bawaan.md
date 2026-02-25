# 💡 AD01.2.3: Graceful Failures: Combining Strategies with Fallbacks

**Outline:**

- **Beyond the Happy Path (Presentation):** Planning for when the network inevitably fails.
- **Handling Errors in Practice (Practice):** Using `setCatchHandler` to define a global fallback.
- **Serving a Custom Offline Page (Production):** Implementing a specific fallback for document requests.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - Beyond the Happy Path**

A robust PWA doesn't just work offline; it handles network _failure_ gracefully. What happens when a user is online, but your server is down? Or they are on a flaky connection and an API request times out? The default browser behavior is to show an error page, which is a jarring and broken user experience.

We can do better. Instead of simply choosing a single caching strategy (like `NetworkFirst`), we can define what should happen _if that strategy fails_. This is the concept of a fallback.

The most common and effective pattern is to provide a pre-cached, custom-designed offline page. For navigation requests, this means if the user tries to go to a page they've never visited while offline, they see your helpful "You're offline" page instead of the browser's dinosaur. For API or image requests, you could fall back to cached placeholder data or a generic image. This transforms a moment of failure into a controlled, on-brand experience.

### **Part 2: Practice - Handling Errors in Practice**

Workbox provides a global error handler called `setCatchHandler`. This is a powerful tool that acts as a safety net for any request that is handled by a Workbox route but ultimately fails.

Let's implement a simple catch handler that just logs the failure. We'll make it more useful in the Production task.

```
// src/sw.ts
// ... (imports)
import { setCatchHandler } from 'workbox-routing';

// ... (all your precaching and registerRoute logic)

// This is our global safety net. It will catch any failed routes.
setCatchHandler(({ event }) => {
  console.error(
    `A Workbox route failed to generate a response.`,
    `Request URL: ${event.request.url}`,
    `Destination: ${event.request.destination}`
  );

  // We MUST return a Response, even an error one.
  return Response.error();
});
```

This code doesn't change the user-facing behavior yet, but it's a critical debugging tool. In your DevTools console, you will now see detailed information whenever one of your caching strategies (like a `NetworkFirst` call that can't reach the network) fails.

**A More Powerful Approach: `warmStrategyCache`** To provide a fallback page, we first need to make sure that page is in our cache and ready to go. We can use the `warmStrategyCache` function from `workbox-recipes` to ensure our offline page is cached on install.

```
// src/sw.ts
import { warmStrategyCache } from 'workbox-recipes';
import { CacheFirst } from 'workbox-strategies';

const offlinePage = '/offline.html';
const strategy = new CacheFirst();
const urls = [offlinePage];

// Warm the cache with our offline page on install
warmStrategyCache({ urls, strategy });

// Now we can use this in a handler...
```

### **Part 3: Production - Serving a Custom Offline Page**

Now let's put it all together. We will create a specific route for all navigation requests. We'll try to get them from the network first. If that fails, we'll serve the pre-warmed `/offline.html` page.

```
// src/sw.ts
// ... (imports)
import { registerRoute } from 'workbox-routing';
import { NetworkOnly } from 'workbox-strategies';
import { warmStrategyCache } from 'workbox-recipes';
import { matchPrecache } from 'workbox-precaching';

// 1. Define and warm the offline page cache
const offlineFallbackPage = '/offline.html';
warmStrategyCache({
  urls: [offlineFallbackPage],
  strategy: new CacheFirst()
});

// 2. Create a route for navigation requests
registerRoute(
  ({ request }) => request.mode === 'navigate',
  // 3. Use a NetworkOnly strategy
  new NetworkOnly({
    // Add a timeout if you want
    // networkTimeoutSeconds: 3
  })
  .catch(async () => {
    // 4. If the network fails, fall back to the precached offline page
    return await matchPrecache(offlineFallbackPage);
  })
);

```

**Breaking it down:**

1. We make sure `/offline.html` is cached and ready.
2. We create a route that only applies to navigation requests.
3. We use `NetworkOnly` because for navigations, we always want the freshest content if possible.
4. We chain a `.catch()` block to the strategy. If the `NetworkOnly` strategy throws an error (because the network is unavailable), this catch block executes.
5. Inside the catch, we use `matchPrecache` to find and return the response for our offline fallback page from the cache.

## 🧠 **Real-World Case Study: "The E-Commerce Checkout Funnel"**

- **Before (No Fallback):** A user is browsing an e-commerce site on their phone. They add items to their cart and tap "Checkout". At that moment, they walk into an elevator and lose their connection. The browser tries to navigate to `/checkout` and fails, showing the "No Internet" page. The user is frustrated, the sale is lost.
- **After (With Fallback):** The developers implement the navigation fallback pattern. When the user taps "Checkout" and loses their connection, the `NetworkOnly` strategy fails. The `.catch()` handler kicks in and serves a pre-cached `/offline-checkout.html` page. This page doesn't have payment processing, but it has a friendly message: "It looks like you're offline. Don't worry, your cart is saved! We'll take you to the final checkout step as soon as you're back online." The user feels reassured, and the sale is likely saved.

## 🤔 **Reflective Questions**

1. Why is it important for the fallback page (`/offline.html`) to be pre-cached?
2. In the example, we used a specific fallback for navigation requests. How might you implement a fallback for a failed API request to `/api/user/profile`? What would you serve instead of an HTML page?
3. What is the difference between `setCatchHandler` and adding a `.catch()` to a specific strategy instance? When would you use each?

## 📚 **Further Reading**

- **Workbox Docs:** [`setCatchHandler`](https://www.google.com/search?q=https://developer.chrome.com/docs/workbox/reference/workbox-routing/%23method-setCatchHandler)
- **Web.dev:** [Handling fallbacks and network errors](https://www.google.com/search?q=https://web.dev/articles/workbox-strategies%23handling-fallbacks-and-network-errors_1)
- **Workbox Recipes:** [`warmStrategyCache`](https://www.google.com/search?q=https://developer.chrome.com/docs/workbox/reference/workbox-recipes/%23method-warmStrategyCache)

## 📝 **Mini Task (Production)**

Create the full offline fallback experience for your app.

1. Create a simple HTML file named `offline.html` in your `public` directory. Style it with a user-friendly message.
2. Ensure `offline.html` is included in your precache manifest (if you're using `injectManifest`, just having it in `public` is usually enough).
3. Implement the full `registerRoute` logic from the "Production" section above to handle failed navigation requests and serve your `offline.html`.
4. Test it: go offline in DevTools and try to navigate to a URL in your app that you have not visited before. You should see your custom offline page instead of a browser error.