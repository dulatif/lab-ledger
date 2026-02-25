### **💡 AA02-3.4: The Manual Cache-First Recipe**

**Outline:**

- **The Metric & The Bottleneck (Presentation):** The need for a reliable, offline-first loading pattern.
- **The Optimization Technique (Practice):** A hands-on guide to implementing the "Cache-First" strategy.
- **Your Performance Mission (Production):** Time to make an image load instantly and work offline.

### 📘 **Full Lesson Content**

### **Part 1: Presentation - The Metric & The Bottleneck**

- **The Goal:** Our goal is to implement one of the most fundamental and powerful caching strategies: **Cache-First**. This will make our application load significantly faster on repeat visits and, crucially, allow it to work offline. We are directly improving **perceived performance** and **network reliability**.
- **Identifying the Problem:** Without a caching strategy, every visit requires the network. If the network is slow, the user waits. If the network is offline, the app is broken. The bottleneck is our dependency on the network for assets that rarely change (like CSS, JS, logos, fonts).

### **Part 2: Practice - The Optimization Technique**

- **The Principle:** The "Cache-First" strategy is simple and declarative:
    
    1. When a request comes in, **first, check the cache.**
    2. If we find a matching response in the cache, serve it immediately. Do not go to the network.
    3. If we **don't** find a match, then (and only then) go to the network to fetch it.
    4. After fetching from the network, put a copy of the response into the cache so that next time, it will be found in step 2.
- **Secure Code Implementation:** This requires two parts: precaching during installation, and the fetch handler.
    
    ```ts
    // in sw.js
    const CACHE_NAME = 'my-site-cache-v1';
    const URLS_TO_CACHE = [
      '/',
      '/styles/main.css',
      '/script/main.js',
      '/images/logo.png'
    ];
    
    // Part 1: Pre-cache the "app shell" during installation
    self.addEventListener('install', event => {
      event.waitUntil(
        caches.open(CACHE_NAME)
          .then(cache => {
            console.log('Opened cache');
            return cache.addAll(URLS_TO_CACHE);
          })
      );
    });
    
    // Part 2: The Cache-First fetch handler
    self.addEventListener('fetch', event => {
      event.respondWith(
        caches.match(event.request)
          .then(response => {
            // If response is found in cache, return it
            if (response) {
              return response;
            }
    
            // Otherwise, go to the network
            return fetch(event.request);
          })
      );
    });
    
    ```
    
    This code makes your app shell load instantly from the cache on every visit after the first one.
    

### 🧠 **Real-World Case Study: "The Unbreakable Web App"**

- **The Scenario:** A field worker uses a web-based application on a tablet to fill out forms. Their mobile connection is often unreliable, dropping in and out as they move around a large worksite.
- **The Problem:** With a standard web app, every time the connection drops, the app becomes unusable. They can't even load the basic form UI to enter data. This makes the web app a non-starter for their job.
- **The Solution:** The developers rebuild the app as a PWA using a Cache-First strategy for the app shell (HTML, CSS, JS).
    1. The first time the worker opens the app (with a good connection), the SW installs and caches the entire UI.
    2. Now, as they move around the site, they can always load the application, even with zero bars of signal. The form UI is served instantly from the cache.
    3. They can fill out the form offline. The app saves the data locally (e.g., in IndexedDB).
    4. When connectivity returns, the app syncs the saved data to the server (using a strategy like Background Sync). The Cache-First strategy transformed an unreliable tool into a mission-critical, offline-capable application.

### 🤔 **Reflective Questions**

1. The code example above never adds _new_ items to the cache after the initial install. How would you modify the `fetch` handler to cache a resource that was fetched from the network? (Hint: `cache.put()` and `response.clone()`).
2. What is the major downside of a pure "Cache-First" strategy? What happens if you update `main.css` on your server? How would a user get that update?
3. Why do we need a versioned `CACHE_NAME` (like `...-v1`)? What part of the SW lifecycle is used to clean up old caches (like `...-v0`)?

### 📚 **Further Reading**

- **MDN:** [Cache API](https://developer.mozilla.org/en-US/docs/Web/API/Cache)
- **web.dev:** [The App Shell Model](https://www.google.com/search?q=https://web.dev/app-shell-model/)
- **Google Devs:** [The Offline Cookbook](https://www.google.com/search?q=https://developer.chrome.com/docs/workbox/caching-strategies-offline-cookbook/)

### 📝 **Mini Task (Production)**

- **Your Mission:** Implement a Cache-First strategy for a specific image.
    1. Start with the code from the "Practice" section.
    2. Modify the `URLS_TO_CACHE` array to include an image, e.g., `/images/my-image.jpg`.
    3. Modify the `fetch` event listener. Instead of handling all requests, add an `if` condition to only apply the Cache-First logic if `event.request.destination` is `'image'`. Let all other requests pass through to the network by default.
    4. Load the page. The first time, the image is fetched from the network and cached.
    5. In DevTools, go to the **Network** tab and check the "Offline" box.
    6. Refresh the page. The page should still load the image successfully, served directly from your SW cache.