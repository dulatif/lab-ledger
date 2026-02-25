# 💡 AD01.2.2: Precision Targeting: Matching with Regex & Callbacks

**Outline:**

- **Beyond Basic Strings (Presentation):** Why we need more powerful matching techniques.
- **Pattern Matching in Practice (Practice):** Using Regular Expressions and custom functions to create routes.
- **Dynamic Caching (Production):** Implementing a route to handle all of your application's images.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - Beyond Basic Strings**

String matching is too rigid for a real-world app. You don't know the exact URL of every image or API call ahead of time. You need to match _patterns_. This is where Workbox's router shines. It allows you to use two powerful tools for matching:

1. **Regular Expressions (Regex):** The classic tool for pattern matching. Perfect for when you can identify a request by a pattern in its URL. Common use cases include:
    - Matching all requests to your API (e.g., anything under `/api/v1/`).
    - Matching all files of a certain type (e.g., all `.jpg`, `.png`, and `.gif` files).
    - Matching requests from a specific CDN or subdomain.
2. **Callback Functions:** For the ultimate level of control. A callback function receives the request object and can inspect _anything_ about it—not just the URL, but also the headers, the request destination, etc. This allows for incredibly sophisticated logic. For example, "match this request if it's a navigation request and the `Accept` header is `text/html`."

Using these tools, you can move from hardcoding URLs to defining broad, intelligent rules that handle entire classes of requests gracefully.

### **Part 2: Practice - Pattern Matching in Practice**

Let's update our `src/sw.ts` with these more advanced techniques.

**Example 1: Using Regex to Cache API Calls**

```ts
// src/sw.ts
// ... (imports)
import { StaleWhileRevalidate } from 'workbox-strategies';
import { registerRoute } from 'workbox-routing';

// ... (precaching logic)

// Use a regular expression to match all requests to our API.
// This will match /api/users, /api/posts, /api/anything.
registerRoute(
  new RegExp('^/api/'), // Matcher: A Regex object
  new StaleWhileRevalidate({
    cacheName: 'api-cache',
  })
);
```

**Example 2: Using a Callback to Cache Images**

```ts
// src/sw.ts
// ... (imports)
import { CacheFirst } from 'workbox-strategies';
import { ExpirationPlugin } from 'workbox-expiration';

// ...

// Use a callback function to match any request that the browser
// identifies as an image request.
registerRoute(
  ({ request }) => request.destination === 'image', // Matcher: A Callback
  new CacheFirst({
    cacheName: 'image-cache',
    plugins: [
      new ExpirationPlugin({
        maxEntries: 60, // Only cache the 60 most recent images
        maxAgeSeconds: 30 * 24 * 60 * 60, // 30 Days
      }),
    ],
  })
);
```

In this second example, we're not even looking at the URL. We're relying on the browser's `request.destination` property, which is a very robust way to identify asset types. We've also added a plugin to keep the cache from growing indefinitely—a crucial practice.

## 🧠 **Real-World Case Study: "The Social Media Feed"**

- **The Problem:** A social media app displays user-uploaded avatars. These avatars are served from a CDN like `https://user-avatars.some-cdn.com/asdf123.jpg`. The URL is unpredictable. The app also loads fonts from Google Fonts and its own API data from `/api/feed`. The team needs a separate caching strategy for each.
- **The Solution (Multiple Advanced Routes):**
    1. **Avatars:** They use a callback: `({url}) => url.origin === '<https://user-avatars.some-cdn.com>'`. They handle this with a `CacheFirst` strategy because avatars don't change often.
    2. **API:** They use a regex: `new RegExp('^/api/')` and handle it with `StaleWhileRevalidate` to keep the feed data fresh but available offline.
    3. **Fonts:** They use another callback: `({url}) => url.origin === '<https://fonts.googleapis.com>'` with a `CacheFirst` strategy.

This multi-route setup creates a highly optimized, resilient experience by treating each asset type appropriately.

## 🤔 **Reflective Questions**

1. What is the advantage of using `request.destination === 'image'` over a regex like `/\\.(png|jpg|svg)$/`?
2. Could you write a callback function that implements the _exact same_ logic as the regex `new RegExp('^/api/')`? What would it look like?
3. When would you absolutely _need_ a callback function instead of a regex? (Hint: Think about what information is available in the `request` object besides the URL).

## 📚 **Further Reading**

- **Workbox Docs:** [Matching a Request with a Regular Expression](https://www.google.com/search?q=https://developer.chrome.com/docs/workbox/modules/workbox-routing/%23matching-a-request-with-a-regular-expression)
- **Workbox Docs:** [Matching a Request with a Callback Function](https://www.google.com/search?q=https://developer.chrome.com/docs/workbox/modules/workbox-routing/%23matching-a-request-with-a-callback-function)
- **MDN:** [`Request.destination`](https://developer.mozilla.org/en-US/docs/Web/API/Request/destination)

## 📝 **Mini Task (Production)**

Your web app is a Single Page Application (SPA). To make it work seamlessly offline, you need to ensure that any navigation attempt (e.g., refreshing the page, or navigating directly to `/dashboard/settings`) always serves your main `index.html` file (which is in your precache). This is the core of the "App Shell" model.

1. In `src/sw.ts`, import the `precache-` and `routing`related modules.
2. Write a `registerRoute` that uses a callback function. The callback should match any request where `request.mode === 'navigate'`.
3. For the handler, use `precache-`'s `createHandlerBoundToURL('/index.html')`. This function creates a handler that will always respond with the pre-cached contents of `/index.html`, no matter what the navigation URL was.
4. Test it by running your built app, navigating to a sub-route like `/some/deep/link`, and then taking your app offline in DevTools and refreshing. It should still load your app shell.