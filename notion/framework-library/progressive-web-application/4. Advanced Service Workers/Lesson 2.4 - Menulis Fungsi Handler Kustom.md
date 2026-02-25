# 💡 AD01.2.4: Full Control: Writing Custom Handler Functions

**Outline:**

- **When Strategies Aren't Enough (Presentation):** Understanding the need for custom logic.
- **Anatomy of a Handler (Practice):** Writing a simple custom handler function from scratch.
- **Synthesizing Responses (Production):** Creating a response on-the-fly within your service worker.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - When Strategies Aren't Enough**

Workbox's built-in strategies (`CacheFirst`, `NetworkFirst`, etc.) are incredible and cover 90% of use cases. But what about the other 10%? Sometimes you need to do something truly custom that doesn't fit neatly into a pre-packaged strategy.

For example, what if you need to:

- Read a value from IndexedDB to decide whether to go to the network or the cache?
- Modify the headers of a request before sending it to the network?
- Stitch together a response from multiple cache entries?
- Generate a response completely from scratch within the service worker itself?

For these scenarios, Workbox allows you to provide your own asynchronous function as a route handler. This function gives you the raw `request` object and expects you to return a `Response` object. Inside that function, you have the full power of the service worker APIs at your disposal. This is the ultimate escape hatch, ensuring that no requirement is too complex for your service worker to handle.

### **Part 2: Practice - Anatomy of a Handler**

A custom handler is just an `async` function that receives an object with useful properties like `request`, `url`, and `event`. Your job is to return a Promise that resolves with a `Response`.

Let's write a very simple handler that just fetches from the network but adds a custom header to the response before it's returned to the page. This is a great way to see how we can intercept and modify things.

```ts
// src/sw.ts
// ... (imports)
import { registerRoute } from 'workbox-routing';

// Our custom handler function
const addCustomHeaderHandler = async ({ request }) => {
  try {
    // 1. Go to the network to get the original response
    const networkResponse = await fetch(request);

    // 2. Clone the response so we can modify it
    const clonedResponse = networkResponse.clone();

    // 3. Create a new Headers object and add our custom header
    const headers = new Headers(clonedResponse.headers);
    headers.append('X-Handled-By', 'Service-Worker');

    // 4. Create a brand new response with the new headers
    const newResponse = new Response(clonedResponse.body, {
      status: clonedResponse.status,
      statusText: clonedResponse.statusText,
      headers: headers,
    });

    // 5. Return our modified response
    return newResponse;

  } catch (error) {
    // If the fetch fails, we could return a fallback response
    console.error('Fetch failed:', error);
    return Response.error();
  }
};

// Use our custom handler for all API requests
registerRoute(new RegExp('^/api/'), addCustomHeaderHandler);

```

**Breaking it down:**

1. We use a standard `fetch(request)` to execute the network request.
2. We **must** clone the response because a `Response` body can only be read once.
3. We create a new `Headers` object based on the original response's headers and add our own.
4. We construct a completely new `Response` using the original body and status, but our new headers.
5. We return this new response. If you inspect the request in DevTools now, you'll see the `X-Handled-By` header!

## 🧠 **Real-World Case Study: "The Dynamic SVG Icon System"**

- **The Problem:** A design application allows users to pick a primary brand color. It needs to show hundreds of SVG icons throughout the UI, all rendered in that brand color. Fetching hundreds of individual SVG files is inefficient. Modifying them all in the DOM with JavaScript is also slow.
- **The Solution (Custom Handler):** The developers create a single route that intercepts all requests for icons (e.g., `/icons/*.svg`). Inside a custom handler, they:
    1. Fetch the base (black) version of the requested SVG icon from the cache.
    2. Read the SVG content as a string.
    3. Use a simple string `replace` to change the `fill="black"` attribute to `fill="#USER_BRAND_COLOR"`. (The brand color is stored in IndexedDB, which the service worker can access).
    4. Create a _new_ `Response` on the fly with the modified SVG string and the correct `Content-Type: image/svg+xml` header.

The result is incredibly efficient. The app only needs to cache one version of each icon, but can serve up dynamically colored versions instantly, with all the heavy lifting done in the background by the service worker.

## 🤔 **Reflective Questions**

1. Why is it mandatory to `clone()` a `Response` object before you can work with its body or headers?
2. A custom handler is an `async` function. What does that imply about what you can do inside it? (Hint: `await`).
3. How could you combine a built-in Workbox strategy with a custom handler? For example, how could you use `CacheFirst` logic inside your own handler?

## 📚 **Further Reading**

- **Workbox Docs:** [Custom Handler Callback](https://www.google.com/search?q=https://developer.chrome.com/docs/workbox/modules/workbox-routing/%23custom-handler-callback)
- **MDN:** [The `Response` object](https://developer.mozilla.org/en-US/docs/Web/API/Response)
- **MDN:** [Using the Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch) (Essential knowledge for writing custom handlers).

## 📝 **Mini Task (Production)**

Create a custom handler that serves a fallback for failed image requests.

1. Pre-cache a placeholder image (e.g., `public/placeholder-image.png`).
2. Register a route that matches image requests (`request.destination === 'image'`).
3. Write a custom handler for this route.
4. Inside the handler, `try` to `fetch` the request from the network.
5. If the fetch is successful, return the network response.
6. In the `catch` block (if the fetch fails), use Workbox's `matchPrecache` function to find and return the placeholder image from your precache.
7. Test it by adding an `<img>` tag in your app that points to a non-existent URL. It should display your placeholder image instead of a broken image icon.