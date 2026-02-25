# 💡 AD01.3.4: The Craftsman: Writing Your Own Custom Plugins

**Outline:**

- **When Off-the-Shelf Isn't Enough (Presentation):** Identifying the need for custom plugin logic.
- **The Plugin Lifecycle (Practice):** Creating a simple plugin with a lifecycle callback.
- **Modifying Responses on the Fly (Production):** Building a plugin that adds a custom header to cached responses.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - When Off-the-Shelf Isn't Enough**

The built-in plugins like `ExpirationPlugin` and `CacheableResponsePlugin` are amazing. But what happens when your app has a very specific, unique requirement?

- What if you need to dynamically modify a response before it gets cached?
- What if you want to broadcast a message to your app every time a request fails in a certain way?
- What if you need to log analytics data about cache hit rates?

This is where you graduate from being a Workbox _user_ to a Workbox _architect_. You can write your own custom plugins. A plugin is simply a JavaScript object that contains functions that match the names of Workbox's lifecycle callbacks. By providing your own implementation for these callbacks, you can execute any code you want at any stage of a strategy's execution. It's the key to unlocking truly advanced, bespoke PWA behaviors.

**Part 2: Practice - The Plugin Lifecycle**

Let's build a simple plugin that demonstrates one of the most common callbacks: `cacheWillUpdate`. This callback is triggered right before a response is about to be written to the cache, giving you a final chance to inspect it or even prevent the cache write entirely.

Our plugin will simply check if a response has a specific header. If it does, we'll block it from being cached.

```ts
// src/sw.ts
// ... (imports)
import { registerRoute } from 'workbox-routing';
import { CacheFirst } from 'workbox-strategies';

// Our custom plugin object
const serverControlPlugin = {
  // The 'cacheWillUpdate' callback
  cacheWillUpdate: async ({ response }) => {
    // Clone the response so we can inspect its headers safely
    const clonedResponse = response.clone();

    // Check for a specific header sent by the server
    if (clonedResponse.headers.has('X-No-Cache')) {
      console.log('Server requested not to cache this response. Rejecting.');
      // By returning null, we tell the strategy to NOT cache the response.
      return null;
    }

    // If the header is not present, return the original response to allow caching.
    return response;
  },
};

// Use our plugin in a route
registerRoute(
  new RegExp('^/api/config/'),
  new CacheFirst({
    cacheName: 'config-cache',
    plugins: [
      serverControlPlugin // Add our custom plugin
    ],
  })
);
```

Now, our service worker respects the server's wishes. If the server sends back `/api/config/settings` with an `X-No-Cache: true` header, our plugin will intercept it and prevent it from poisoning our `CacheFirst` cache.

## **🧠 Real-World Case Study: "Tracking Offline Usage"**

- **The Problem:** A product team wants to know how often their PWA serves content from the cache. They want to track "cache hits" in their analytics platform to measure the effectiveness of their offline strategy. The built-in strategies don't provide a hook for this.
    
- **The Solution (Custom Plugin):** An engineer writes a simple custom plugin with one callback: `cachedResponseWillBeUsed`. This callback is only triggered when a strategy successfully finds a response in the cache and is about to use it.
    
    ```ts
    const analyticsPlugin = {
      cachedResponseWillBeUsed: async ({ cacheName, request }) => {
        // This code runs on every cache hit
        console.log(`Serving ${request.url} from ${cacheName}.`);
        // In a real app, you'd post this data to your analytics endpoint
        // e.g., googleAnalytics.trackEvent('Cache', 'Hit', request.url);
      }
    };
    ```
    

They can now add this `analyticsPlugin` to _every single route_ in their service worker. It's a clean, reusable, and powerful way to add cross-cutting analytics logic to their entire caching architecture.

## 🤔 **Reflective Questions**

1. Look at the list of plugin lifecycle callbacks in the Workbox docs. Which callback would you use to modify a `Request` _before_ it's sent to the network?
2. What is the key difference between writing a custom plugin and writing a custom handler function? When is a plugin the better architectural choice?
3. Why is it important to `clone()` the `response` inside a callback before you read from it (e.g., check its headers or body)?

## 📚 **Further Reading**

- **Workbox Docs:** [The complete list of plugin lifecycle callbacks](https://www.google.com/search?q=https://developer.chrome.com/docs/workbox/reference/workbox-core/%23iface-WorkboxPlugin) (Essential reading).
- **Workbox Docs:** [Writing a Custom Plugin](https://www.google.com/search?q=https://developer.chrome.com/docs/workbox/using-plugins%23writing-a-custom-plugin) (Official guide).
- **A Deep Dive Article:** [Custom Workbox Plugins for advanced PWA features](https://www.google.com/search?q=https://medium.com/samsung-internet-dev/custom-workbox-plugins-for-advanced-pwa-features-593b5827b5e6) (Provides more complex examples).

## 📝 **Mini Task (Production)**

Create a custom plugin that adds a header to a response right before it's cached. This is useful for debugging, as you can see in DevTools whether a response was "touched" by the service worker.

1. Create a custom plugin object named `addHeaderPlugin`.
2. Implement the `cacheWillUpdate` callback.
3. Inside the callback, do the following:
    - Clone the incoming `response`.
    - Create a new `Headers` object from the cloned response's headers.
    - Append a new header: `X-Cached-By: 'Service-Worker'`.
    - Return a new `Response` object, passing in the cloned response's body and status, but your _new_ headers object.
4. Apply this plugin to your image caching route.
5. Test it. Load a new image, then go to the "Application" tab in DevTools, find your image cache, click on the cached response for the new image, and verify that your custom header is present.