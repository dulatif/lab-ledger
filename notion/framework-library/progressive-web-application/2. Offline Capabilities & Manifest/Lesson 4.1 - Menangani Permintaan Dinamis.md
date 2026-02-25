# 💡 CM01-4.1: Beyond the Shell: Handling Dynamic Requests

**Outline:**

- **The Two Halves of Your App (Presentation):** Understanding the crucial difference between precaching the app shell and runtime caching dynamic content.
- **The Traffic Cop (Practice):** Introducing the `registerRoute` function, Workbox's tool for intercepting live requests.
- **Setting the Trap (Production):** Time to write your first route-handling rule to intercept API calls.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Two Halves of Your App**

So far, we've mastered **precaching**. We've told Workbox, "Here is my App Shell, my core set of files. Cache these upfront so my app always loads." This is perfect for predictable, static assets.

But what about the unpredictable?

- API calls to fetch data (`/api/users`, `/api/news`)
- Images uploaded by users
- Third-party assets like Google Fonts or analytics scripts

These requests happen while the user is _using_ the app. We can't precache them because we don't know what the user will ask for. This is where **runtime caching** comes in.

**The Core Philosophy:** Runtime caching intercepts requests as they happen live. We define rules that tell the service worker, "When you see a request that looks like _this_ (e.g., an API call), handle it using _that_ caching strategy." This allows us to build a fast, offline-capable experience for the dynamic parts of our application, not just the static shell. Precaching is for the shell; runtime caching is for the content.

### **Part 2: Practice - The Traffic Cop**

The main tool Workbox gives us for this is `registerRoute`. It acts like a traffic cop for network requests, taking two main arguments:

1. **A "Matcher":** A function that inspects a request and returns `true` if this is a request we want to handle. You can match based on the URL, the request destination (is it an image, a script?), headers, etc.
2. **A "Handler":** The caching strategy to use if the matcher returns `true`. (We'll cover the different strategies in the next lessons).

Here's how you would set up a rule in a custom service worker file (`sw.js`). You can also configure this within the `vite-plugin-pwa` options.

```tsx
// This code would live in a custom service worker file,
// or be configured via your build plugin.

import { registerRoute } from 'workbox-routing';
import { NetworkFirst } from 'workbox-strategies'; // A strategy we'll learn soon

// The Matcher function
const apiMatcher = ({ url }) => {
  return url.pathname.startsWith('/api/');
};

// The Handler (a caching strategy)
const apiHandler = new NetworkFirst({ cacheName: 'api-cache' });

// Putting it all together
registerRoute(apiMatcher, apiHandler);
```

This code tells Workbox: "For any request whose URL path starts with `/api/`, apply the `NetworkFirst` strategy and store it in a cache named `api-cache`."

## 🧠 **Real-World Case Study: A Weather PWA**

- **Precaching:** The app precaches its shell: the HTML, CSS, the logo image, and the JavaScript to render the UI. This makes the app load instantly.
- **Runtime Caching:** When the user searches for a city, the app makes a request to `https://api.weather.com/forecast?q=London`. A `registerRoute` rule in the service worker sees this outgoing request. It matches the URL. It then applies a caching strategy (like `StaleWhileRevalidate`) to cache the forecast data. The next time the user opens the app offline, the shell loads from the precache, and the last-viewed forecast can be served from the runtime cache.

## 🤔 **Reflective Questions**

1. Why would it be a very bad idea to add API endpoints (like `/api/get-all-posts`) to your precache manifest?
2. Besides the URL, what other properties of a request object could you use for matching a route? (e.g., `request.method`, `request.headers`).
3. What is the main difference in _when_ precaching happens versus _when_ runtime caching happens?

## 📚 **Further Reading**

- **Workbox Docs on Routing:** [Routing with Workbox](https://developer.chrome.com/docs/workbox/modules/workbox-routing/)
- **Web.dev Caching Patterns:** [An overview of caching patterns](https://www.google.com/search?q=https://web.dev/patterns/caching/)

## 📝 **Mini Task (Production)**

Let's set up our first interception rule. This will require a bit more setup if you've been using the auto-generated SW. The best way is to tell `vite-plugin-pwa` you want to inject your own code.

**Task:**

1. In your `vite.config.ts`, change your PWA plugin configuration to use a custom service worker file. You will need to create this file.
    
    ```ts
    // vite.config.ts
    VitePWA({
      strategies: 'injectManifest', // Use 'injectManifest' instead of 'generateSW'
      srcDir: 'src', // Location of your service worker source
      filename: 'sw.ts' // The name of your service worker source file
    })
    
    ```
    
2. Create a new file at `src/sw.ts`.
    
3. Add the following code to your new `src/sw.ts`. This code uses Workbox to handle precaching (just like before) and adds a simple route to catch API calls.
    
    ```ts
    // src/sw.ts
    import { cleanupOutdatedCaches, precacheAndRoute } from 'workbox-precaching'
    import { registerRoute } from 'workbox-routing'
    
    declare let self: ServiceWorkerGlobalScope
    
    // The __WB_MANIFEST placeholder will be replaced by the build plugin
    precacheAndRoute(self.__WB_MANIFEST)
    
    cleanupOutdatedCaches()
    
    // Our first runtime route!
    registerRoute(
      ({ url }) => url.pathname.startsWith('/api/'),
      async ({ request }) => {
        console.log('Intercepted an API request!', request.url);
        // For now, just pass it through to the network
        return fetch(request);
      }
    );
    ```
    
4. Run `npm run build` and `serve -s dist`. When you load your app and it makes an API call, you should see your `console.log` message in the browser's developer console.