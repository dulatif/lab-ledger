### **💡 AA02-4.3: Cache-First vs. Network-First: Choosing Your Weapon**

**Outline:**

- **The Metric & The Bottleneck (Presentation):** The risk of applying a one-size-fits-all caching strategy.
- **The Optimization Technique (Practice):** A hands-on guide to implementing Cache-First and Network-First strategies.
- **Your Performance Mission (Production):** Time to build a robust routing strategy for different asset types.

### 📘 **Full Lesson Content**

### **Part 1: Presentation - The Metric & The Bottleneck**

- **The Goal:** Our goal is to make intelligent, deliberate decisions about caching. We need to match the caching strategy to the resource's lifecycle to optimize for both **speed** (for immutable assets) and **reliability** (for critical, dynamic content).
- **Identifying the Problem:** The bottleneck is a naive caching approach. If you use Cache-First for everything, your users might get stuck with an old version of your application's HTML. If you use Network-First for everything, you lose the instant-load benefit for static assets like fonts and logos. A single strategy is never the right answer.

### **Part 2: Practice - The Optimization Technique**

- **The Principle:** We must choose the right tool for the job. Workbox provides two strategies for these explicit use cases.
    
    **1. Cache-First**
    
    - **How it Works:** Check the cache. If found, serve it. If not, go to network, serve it, and cache it.
        
    - **Best For:** Immutable assets. Things that, if they change, will have a new URL (e.g., versioned files).
        
    - **Examples:** Fonts, logos, third-party libraries, JS/CSS bundles with a content hash in the filename (`app.[hash].js`).
        
    - **Workbox Code:**
        
        ```ts
        routing.registerRoute(
          ({ request }) => request.destination === 'font' || request.destination === 'image',
          new strategies.CacheFirst({
            cacheName: 'asset-cache',
          })
        );
        
        ```
        
    
    **2. Network-First**
    
    - **How it Works:** Go to the network. If it succeeds, serve it and update the cache. If it fails (offline), fall back to the last cached version.
        
    - **Best For:** Resources that need to be as fresh as possible, but where having _some_ content (even if slightly stale) is better than a browser error page.
        
    - **Examples:** The HTML for an article page, a user's profile information.
        
    - **Workbox Code:**
        
        ```ts
        routing.registerRoute(
          ({ request }) => request.mode === 'navigate', // e.g., HTML pages
          new strategies.NetworkFirst({
            cacheName: 'page-cache',
          })
        );
        
        ```
        

### 🧠 **Real-World Case Study: "The Guardian PWA"**

- **The Scenario:** The Guardian's Progressive Web App is a premier example of a content site that works beautifully offline.
- **The Architecture:** They employ a multi-strategy approach.
    - **App Shell (logo, global CSS, core JS):** These are served using a **Cache-First** strategy. They are versioned and are precached when the Service Worker installs. This makes the application frame load instantly.
    - **Article Pages (HTML):** When you click an article link, they use a **Network-First** strategy. This ensures you get the very latest version of the article if you're online. But if you're on the subway and lose connection, it falls back to the version of the article that was cached the last time you viewed it.
    - **Article Images:** These often use a **Stale-While-Revalidate** or **Cache-First** strategy with an expiration policy.
- **The Result:** An app that feels incredibly fast, is always up-to-date when possible, and remains completely functional when offline.

### 🤔 **Reflective Questions**

1. Describe the user experience of a `NetworkFirst` strategy when the user is online but on a very slow (e.g., 10 seconds to respond) network.
2. Why is `CacheFirst` a potentially dangerous strategy for your main `index.html` file if its name never changes?
3. Imagine you have a "Save for Offline" button on an article. What Workbox strategy does this manual action most closely resemble?

### 📚 **Further Reading**

- **Workbox Docs:** [`CacheFirst` Strategy](https://www.google.com/search?q=https://developer.chrome.com/docs/workbox/modules/workbox-strategies/%23cache-first)
- **Workbox Docs:** [`NetworkFirst` Strategy](https://www.google.com/search?q=https://developer.chrome.com/docs/workbox/modules/workbox-strategies/%23network-first)
- **Google Devs:** [The Offline Cookbook](https://www.google.com/search?q=https://developer.chrome.com/docs/workbox/caching-strategies-offline-cookbook/)

### 📝 **Mini Task (Production)**

- **Your Mission:** You are building the Service Worker for a blog. Your task is to implement a robust routing configuration.
    1. Set up a Workbox Service Worker.
    2. Implement a `CacheFirst` strategy for all requests for images (`request.destination === 'image'`).
    3. Implement a `NetworkFirst` strategy for all navigation requests (`request.mode === 'navigate'`).
    4. Implement a `StaleWhileRevalidate` strategy for all CSS and JS files (`request.destination` of `'style'` or `'script'`).
    5. Test your implementation by checking the different caches created in the DevTools Application tab.