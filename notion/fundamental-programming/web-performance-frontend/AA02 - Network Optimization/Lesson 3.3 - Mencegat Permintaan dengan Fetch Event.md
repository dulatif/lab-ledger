### **💡 AA02-3.3: Intercepting Requests with the Fetch Event**

**Outline:**

- **The Metric & The Bottleneck (Presentation):** The mechanics of intercepting a request and the necessity of responding.
- **The Optimization Technique (Practice):** A hands-on guide to using `event.respondWith()` to control the network.
- **Your Performance Mission (Production):** Time to hijack a CSS request.

### 📘 **Full Lesson Content**

### **Part 1: Presentation - The Metric & The Bottleneck**

- **The Goal:** Our goal is to gain practical, code-level control over network requests using the `fetch` event listener. This is the core mechanism that enables all advanced caching, offline, and network-resilience strategies.
- **Identifying the Problem:** The bottleneck in understanding is the `event.respondWith()` method. The `fetch` event is special. If you don't call `event.respondWith()` with a `Promise` that resolves to a `Response`, the browser assumes you didn't handle the request, and it simply proceeds to the network as if the SW didn't exist. Forgetting this method or using it incorrectly means your SW does nothing.

### **Part 2: Practice - The Optimization Technique**

- **The Principle:** The `fetch` event listener receives a `FetchEvent` object. This object contains the `request` that was made. Our job is to provide a `Response` for that request.
    
- **Secure Code Implementation:**
    
    **Step 1: The "Pass-Through" Proxy** This is the simplest useful SW. It intercepts every request, logs it, and then forwards it to the network.
    
    ```ts
    // in sw.js
    self.addEventListener('fetch', event => {
      console.log('Intercepting request for:', event.request.url);
    
      // event.respondWith() takes a Promise that resolves to a Response.
      // fetch() returns exactly that!
      event.respondWith(fetch(event.request));
    });
    
    ```
    
    **Step 2: The "Hijacker" Proxy** Let's intercept a specific request and return our own, completely fabricated response.
    
    ```ts
    self.addEventListener('fetch', event => {
      // Check if the request is for our main image
      if (event.request.url.endsWith('/images/hero.jpg')) {
        console.log('Hijacking the hero image request!');
    
        // Don't go to the network.
        // Instead, respond with a programmatically created SVG image.
        event.respondWith(
          new Response(
            '<svg viewBox="0 0 100 100" xmlns="<http://www.w3.org/2000/svg>"><circle cx="50" cy="50" r="50" fill="orange"/></svg>',
            { headers: { 'Content-Type': 'image/svg+xml' } }
          )
        );
      }
      // For all other requests, do nothing, and they will go to the network by default.
    });
    
    ```
    
    In this example, the page's `<img src="/images/hero.jpg">` would display an orange circle, even though that SVG exists nowhere on the server.
    

### 🧠 **Real-World Case Study: "The Custom Offline Page"**

- **The Scenario:** A user tries to navigate to a page on a news website while they are offline.
    
- **The Problem:** The browser makes a navigation request for the page's HTML. The request fails, and the user sees the browser's default, ugly "You are offline" dinosaur page. This is a poor user experience.
    
- **The Solution:** The developer's Service Worker intercepts the request.
    
    ```ts
    self.addEventListener('fetch', event => {
      // Only intercept navigation requests
      if (event.request.mode === 'navigate') {
        event.respondWith(
          // Try the network first
          fetch(event.request).catch(() => {
            // If the network fails, respond with a pre-cached offline fallback page.
            return caches.match('/offline.html');
          })
        );
      }
    });
    
    ```
    
    Now, when a user tries to navigate while offline, the `fetch` fails, and the SW's `.catch()` block is executed. It serves a nicely designed, pre-cached `/offline.html` page that tells the user they're offline and can try again later. The experience feels like part of the application, not a browser error.
    

### 🤔 **Reflective Questions**

1. What is the `Response` object? What are the two main parts you need to construct one?
2. In the first example, if the network request in `fetch(event.request)` fails, what happens to the user? How could you make that more robust?
3. Why is it important to check `event.request.url` or `event.request.mode`? What would happen if you applied a single caching strategy to _every_ request, including API calls and analytics pings?

### 📚 **Further Reading**

- **MDN:** [`FetchEvent`](https://developer.mozilla.org/en-US/docs/Web/API/FetchEvent)
- **MDN:** [`FetchEvent.respondWith()`](https://developer.mozilla.org/en-US/docs/Web/API/FetchEvent/respondWith)
- **web.dev:** [An offline fallback page](https://web.dev/offline-fallback-page/)

### 📝 **Mini Task (Production)**

- **Your Mission:** You are given a simple web page that loads a stylesheet at `/styles.css`. Your task is to use a Service Worker to intercept this request and change the page's background color.
    1. Set up a basic `index.html`, `/styles.css`, and `sw.js`.
    2. In `sw.js`, add a `fetch` event listener.
    3. Inside the listener, add an `if` statement to check if `event.request.url` contains `/styles.css`.
    4. If it does, use `event.respondWith()` to return a `new Response()`.
    5. The body of the response should be the string `'body { background-color: hotpink; }'`.
    6. Remember to include the correct `Content-Type` header: `{ headers: { 'Content-Type': 'text/css' } }`.
    7. Load the page. It should have a hotpink background, served directly from your Service Worker.