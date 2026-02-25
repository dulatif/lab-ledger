# 💡 AD01.3.2: The Gatekeeper: Caching Only "Good" Responses

**Outline:**

- **The Poisoned Cache (Presentation):** Understanding the danger of caching failed network responses.
- **Using the Gatekeeper (Practice):** Implementing the `CacheableResponsePlugin`.
- **Building a Resilient API Cache (Production):** Applying the plugin to a real-world API route.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Poisoned Cache**

Imagine this scenario: your app makes a request to `/api/user/profile`. Your server has a temporary issue and responds with a `500 Internal Server Error`. Your service worker, trying to be helpful, caches this `500` response. Now you have a **poisoned cache**.

What happens next? The server recovers a minute later, but your PWA is using a `CacheFirst` strategy. The next time the user visits their profile, the service worker finds the `500` response in the cache and serves it _instantly_. The user is now stuck looking at a broken page, even though the server is perfectly healthy. They are trapped by your well-intentioned but naive caching logic.

We must teach our service worker to be more discerning. It should act as a gatekeeper, inspecting responses from the network and only allowing "good" ones into the cache. A "good" response is typically one with a `200 OK` status, but it could also include redirects (`3xx`) in some cases. Caching errors is almost always a bad idea.

### **Part 2: Practice - Using the Gatekeeper**

Workbox gives us the perfect tool for this job: the `CacheableResponsePlugin`. It's a simple plugin that lets you define exactly which responses are worthy of being cached.

Let's set up a route for a third-party API and ensure we only cache successful responses.

```ts
// src/sw.ts
// ... (imports)
import { registerRoute } from 'workbox-routing';
import { NetworkFirst } from 'workbox-strategies';
import { CacheableResponsePlugin } from 'workbox-cacheable-response'; // 1. Import the plugin

// ... (precaching logic)

registerRoute(
  ({ url }) => url.origin === '<https://api.thirdparty.com>',
  new NetworkFirst({
    cacheName: 'third-party-api-cache',
    plugins: [
      // 2. Add and configure the plugin
      new CacheableResponsePlugin({
        statuses: [0, 200], // Cache successful responses and opaque responses (status 0)
      }),
    ],
  })
);
```

**Breaking it down:**

1. We import the `CacheableResponsePlugin`.
2. We add it to our strategy's `plugins` array.
3. We configure it with the `statuses` property. This tells the plugin: "Only allow a response to be cached if its `status` code is in this array."
    - `200`: The standard code for a successful request.
    - `0`: This is the status for an "opaque response"—a successful cross-origin request made in `no-cors` mode. While you can't read the body or status of these, caching them is often necessary for third-party assets.

Now, if a request to `api.thirdparty.com` returns a `404 Not Found` or `503 Service Unavailable`, the `NetworkFirst` strategy will fail to get a valid network response, but more importantly, the plugin will prevent the bad response from ever being written to the cache.

## 🧠 **Real-World Case Study: "The Missing Avatar"**

- **Before (No Plugin):** A social media app requests a user's avatar at `/avatars/user123.png`. However, the user hasn't uploaded one, so the server correctly returns a `404 Not Found`. The app's service worker, using a `CacheFirst` strategy, caches this `404` response. A week later, the user uploads an avatar. But every time the app loads their profile, the service worker finds the `404` in the cache and serves it, showing a broken image. The app is stuck serving a "not found" response even though the resource now exists.
- **After (With Plugin):** The developers add `new CacheableResponsePlugin({ statuses: [200] })` to their image caching route. When the initial request for the avatar returns a `404`, the plugin inspects the response and blocks it from being cached. The next time the profile is loaded, the service worker doesn't find anything in the cache for that URL, so it goes to the network. This time, it finds the newly uploaded image, gets a `200 OK` response, and the plugin allows this successful response to be cached for future offline use.

## 🤔 **Reflective Questions**

1. You can also configure `CacheableResponsePlugin` with a `headers` property (e.g., `headers: {'X-Is-Cacheable': 'true'}`). When would this be more useful than matching on status codes?
2. What is an "opaque response" and why might you want to cache it, even though you can't read its contents?
3. If you _don't_ use this plugin, what is the default behavior of Workbox strategies regarding caching responses with error statuses?

## 📚 **Further Reading**

- **Workbox Docs:** [`CacheableResponsePlugin`](https://www.google.com/search?q=https://developer.chrome.com/docs/workbox/reference/workbox-cacheable-response/classes/CacheableResponsePlugin)
- **MDN:** [HTTP response status codes](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status) (A good refresher).
- **Google Web Fundamentals:** [An introduction to Cross-Origin Resource Sharing (CORS)](https://web.dev/articles/cross-origin-resource-sharing) (Explains why some responses are "opaque").

## 📝 **Mini Task (Production)**

Your app has a route that handles caching for images from a CDN. This CDN sometimes returns a `202 Accepted` status for images that are still being processed. You want to cache `200` responses but not `202` responses.

1. Locate the route in your `src/sw.ts` that handles image caching (or create one).
2. Ensure it uses the `CacheableResponsePlugin`.
3. Configure the plugin to _only_ cache responses with a status of `200`. (Do not include `0` for this exercise).
4. Add a code comment explaining why caching a `202` response for an image could lead to a poor user experience.