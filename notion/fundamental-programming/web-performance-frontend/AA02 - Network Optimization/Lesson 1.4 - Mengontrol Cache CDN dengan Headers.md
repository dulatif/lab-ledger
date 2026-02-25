### **💡 AA02-1.4: Taking Control: CDN Caching with Headers**

**Outline:**

- **The Metric & The Bottleneck (Presentation):** The risk of serving stale content or having a low cache hit ratio.
- **The Optimization Technique (Practice):** A hands-on guide to the `Cache-Control` header.
- **Your Performance Mission (Production):** Time to analyze the caching strategy of a real-world asset.

### 📘 **Full Lesson Content**

### **Part 1: Presentation - The Metric & The Bottleneck**

- **The Goal:** Our goal is to explicitly tell CDNs (and browsers) how to cache our assets. We want to maximize the **Cache Hit Ratio**—the percentage of requests served from the cache—without serving out-of-date content.
- **Identifying the Problem:** A CDN without proper instructions is inefficient. The bottleneck is uncertainty. If we don't tell the CDN how long it's safe to cache a file, it might have to re-validate with the origin server on every request, which reduces the performance gain. Or worse, it might cache something for too long. Imagine you push a critical CSS bug fix, but the CDN continues to serve the old, broken stylesheet for 24 hours to all your users. This is a cache configuration failure.

### **Part 2: Practice - The Optimization Technique**

- **The Principle:** We control caching behavior using an HTTP response header sent from our origin server: `Cache-Control`. This header contains directives that instruct all caches (CDN, browser, etc.) on how to handle the response.
    
- **Key `Cache-Control` Directives:**
    
    - `public`: The response can be stored by any cache (like a CDN).
    - `private`: The response is for a single user and shouldn't be stored in a shared cache (e.g., a user's account page).
    - `max-age=<seconds>`: How long the browser can cache the file.
    - `s-maxage=<seconds>`: (The 's' is for 'shared') How long a shared cache, like a CDN, can cache the file. This overrides `max-age` for CDNs.
    - `stale-while-revalidate=<seconds>`: A powerful directive. It tells the cache it can serve a stale (expired) asset immediately to the user, while it re-fetches a fresh one in the background. This is the mechanism that powers ISR.
- **Example Configurations (in a Next.js API route or `getServerSideProps`):**
    
    ```
    // For a static asset that never changes (versioned file)
    // Cache for 1 year on the CDN and in the browser
    res.setHeader('Cache-Control', 'public, s-maxage=31536000, max-age=31536000, immutable');
    
    // For a dynamic API response that should be fresh
    // Cache on the CDN for 1 minute, revalidate in background
    res.setHeader('Cache-Control', 'public, s-maxage=60, stale-while-revalidate=120');
    
    ```
    

### 🧠 **Real-World Case Study: "The Un-cacheable CSS"**

- **The Scenario:** A developer deploys their site. They notice that even though it's on Vercel, the TTFB for their main CSS file seems slow, and the `x-vercel-cache` header often shows `MISS`.
- **The Problem:** They inspect the response headers for `main.css` and find `Cache-Control: no-cache`. Their server is explicitly telling the CDN _not_ to cache the file. Every single user request is going back to the origin server to fetch the CSS file. Their Cache Hit Ratio is 0%.
- **The Solution:** The developer realizes their build system is not adding version hashes to their filenames (e.g., `main.[hash].css`). Because the filename never changes, they were afraid of caching it. They fix their build process to include content hashes. Now, when the CSS changes, the filename changes. They can now safely apply a very aggressive caching header to their CSS files: `Cache-Control: public, max-age=31536000, immutable`. The `x-vercel-cache` header now always shows `HIT`, and the CSS file loads instantly from the edge.

### 🤔 **Reflective Questions**

1. What is the difference between `max-age` and `s-maxage`? Why is this distinction so important for CDNs?
2. The `immutable` directive tells the browser that the file will _never_ change. When is it safe to use this, and what is the key prerequisite for using it?
3. How does the `stale-while-revalidate` directive provide a great balance between performance and content freshness?

### 📚 **Further Reading**

- **MDN:** [`Cache-Control` Header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Cache-Control)
- **web.dev:** [HTTP Caching](https://web.dev/http-cache/)
- **Vercel:** [Vercel Caching Headers](https://vercel.com/docs/edge-network/caching)

### 📝 **Mini Task (Production)**

- **Your Mission:** Let's inspect the Google Fonts CSS file, a perfect example of a highly-cached asset.
    1. In Chrome, open DevTools -> **Network** tab.
    2. Visit this URL: `https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap`
    3. Click on the request in the Network panel.
    4. Look at the `cache-control` response header.
    5. Write down the value of the header. Based on the directives you see, explain in one sentence what caching strategy Google is instructing CDNs and browsers to use for this file.