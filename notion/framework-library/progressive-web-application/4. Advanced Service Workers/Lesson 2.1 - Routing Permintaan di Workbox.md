# 💡 AD01.2.1: The Traffic Cop: Routing Requests in Workbox

**Outline:**

- **The Router's Role (Presentation):** Understanding how Workbox intercepts and directs network requests.
- **Registering a Basic Route (Practice):** A hands-on guide to your first routing rule.
- **Separating Concerns (Production):** Applying specific caching to different asset types.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Router's Role**

So far, we've dealt with precaching (our app shell) and a simple runtime cache. Now, let's get serious about managing network traffic. Think of the Workbox Router as the traffic cop for your application. Every single outgoing request, whether it's for an image, a font, an API call, or a page navigation, passes through it.

The Router's job is to look at each request and ask, "Do I have a specific rule for this?" It does this by checking a list of _routes_ that you've registered.

Each **Route** has two parts:

1. **A Matcher:** The condition that a request must meet for the route to apply (e.g., "is the URL a Google Font?").
2. **A Handler:** The action to take if the matcher returns true (e.g., "use the `CacheFirst` strategy").

If a request matches a route, that route's handler takes over. If no routes match, the request is passed through to the network as if the service worker wasn't even there. By defining these routes, we gain granular control and can build a PWA that is intelligent about how it handles the network.

### **Part 2: Practice - Registering a Basic Route**

Let's start with the simplest type of matcher: a string. This is great for an exact match of a specific file. While less common in complex apps, it's perfect for understanding the core mechanism.

In your `src/sw.ts`, let's add a specific route for a global stylesheet.

```ts
// src/sw.ts

// ... (imports and precaching logic)
import { registerRoute } from 'workbox-routing';
import { StaleWhileRevalidate } from 'workbox-strategies';

// ... (precacheAndRoute, cleanupOutdatedCaches)

// Let's add our first specific route.
// This will match any request made specifically to '/global-styles.css'
registerRoute(
  '/global-styles.css', // The Matcher: an exact string
  new StaleWhileRevalidate({ // The Handler: a caching strategy
    cacheName: 'stylesheets',
  })
);
```

Here, we've told the router: "Anytime the app requests the file at the path `/global-styles.css`, I want you to apply the `StaleWhileRevalidate` strategy and store it in a cache named 'stylesheets'". It's explicit, readable, and powerful. We're now managing this file's cache life cycle independently from our pre-cached assets.

## 🧠 **Real-World Case Study: "The A/B Testing Script"**

- **The Problem:** A marketing team wants to run A/B tests using a third-party script (e.g., from `cdn.optimizely.com/script.js`). They want this script to be fresh to always get the latest experiment data, but they don't want it to slow down the site if the CDN is slow. Caching it forever (`CacheFirst`) is bad because tests won't update. Not caching it is bad for performance.
- **The Solution (`StaleWhileRevalidate` Route):** The engineering team adds a specific route to `sw.ts`: `registerRoute('<https://cdn.optimizely.com/script.js>', new StaleWhileRevalidate({ cacheName: 'ab-testing-scripts' }))`. This provides the perfect balance. The app serves the cached script instantly for fast performance, but always re-fetches it in the background to ensure the marketing team's latest A/B tests are applied on the user's next visit.

## 🤔 **Reflective Questions**

1. What happens to a request for `/main.js` if the only registered route is for `/global-styles.css`?
2. Why is string matching not ideal for caching a collection of assets, like all JPEG images in your app?
3. If you have two routes that could potentially match the same request, which one does the Workbox Router choose? (Hint: check the docs!)

## 📚 **Further Reading**

- **Workbox Docs:** [The `registerRoute` function](https://www.google.com/search?q=https://developer.chrome.com/docs/workbox/reference/workbox-routing/%23method-registerRoute)
- **Web.dev:** [Workbox Routing](https://www.google.com/search?q=https://web.dev/articles/workbox-routing) (A great high-level overview).
- **Workbox Strategies:** [Core Caching Strategies](https://developer.chrome.com/docs/workbox/modules/workbox-strategies)

## 📝 **Mini Task (Production)**

Your application uses a third-party analytics script located at `https://cdn.example.com/analytics.js`. You want to cache this script to improve performance, but you don't want to risk serving a stale version that might misreport data.

1. In your `src/sw.ts`, import the `NetworkFirst` strategy from `workbox-strategies`.
2. Register a new route that specifically matches the full URL of the analytics script.
3. Use the `NetworkFirst` strategy as the handler, configuring it to use a cache named `analytics-cache`.
4. Explain in a code comment why `NetworkFirst` is a suitable choice for an analytics script compared to `CacheFirst`.