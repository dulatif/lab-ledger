### **💡 AE01-2.4: Do No Harm: A Performance-First RUM Implementation**

**Outline:**

- **The Metric & The Bottleneck (Presentation):** The irony of a performance script hurting performance.
- **The Optimization Technique (Practice):** A checklist for implementing RUM without impacting users.
- **Your Performance Mission (Production):** Time to refactor your implementation to be non-blocking.

### 📘 **Full Lesson Content**

### **Part 1: Presentation - The Metric & The Bottleneck**

- **The Goal:** Our goal is to ensure our Real User Monitoring script adheres to the principle of "do no harm." The script must collect data with the absolute minimum possible impact on the user's experience and the metrics we are trying to measure.
- **Identifying the Problem:** This is the **Observer Effect**. The very act of measuring a system can alter it. If your RUM script is large, synchronous, or blocks the main thread, it can directly worsen your LCP, INP, and overall page load time. The bottleneck is an **unoptimized implementation** that pollutes the very data it's supposed to be collecting. A poorly implemented RUM script is worse than no RUM script at all.

### **Part 2: Practice - The Optimization Technique**

- **The Principle:** We must treat our RUM script as a third-class citizen. It should be the last thing to load and the last thing to execute, getting out of the way of critical user-facing content.
- **The Performance Checklist:**
    1. **Load Asynchronously:** Never, ever load your analytics script with a blocking `<script>` tag. Always use `async` or `defer`. `async` is often preferred for analytics as it doesn't block the HTML parser at all.
        
        ```html
        <!-- Good -->
        <script async src="/js/vitals-collector.js"></script>
        
        <!-- Bad! Blocks rendering! -->
        <script src="/js/vitals-collector.js"></script>
        
        ```
        
    2. **Keep The Bundle Small:** The `web-vitals` library is already very small (~1.5kB), which is excellent. When building your own collector, be ruthless about its size. It should not contain a huge framework or have many dependencies.
        
    3. **Don't Bundle It:** Avoid importing your vitals collection logic directly into your main application bundle (`main.js`). This unnecessarily bloats the bundle that is critical for rendering your app. Loading it as a separate, async script is a much better pattern.
        
    4. **Use Idle Callbacks:** For any non-essential work (like initializing the script or preparing data), consider wrapping it in `requestIdleCallback` to ensure it only runs when the main thread is free.
        
        ```ts
        if ('requestIdleCallback' in window) {
          requestIdleCallback(initializeAnalytics);
        } else {
          // Fallback for older browsers
          setTimeout(initializeAnalytics, 1);
        }
        
        ```
        

### 🧠 **Real-World Case Study: "The Self-Inflicted Wound"**

- **The Scenario:** A company decides to build its own RUM solution. The developers import the `web-vitals` library and all their data-sending logic directly into their main `App.js` component in their React application.
- **The Problem:** After deploying, their RUM data shows a 200ms regression in LCP across the board. They are confused because they haven't changed any UI code.
- **The Cause:** By adding the analytics code to their main bundle, they increased its gzipped size by 2KB. While small, this was enough to tip them into the next network round trip for some users. More importantly, it added extra JavaScript for the browser to parse and execute on the main thread _before_ it could start rendering the main application content. Their RUM script was literally blocking their LCP. They fixed it by pulling the logic out and loading it via an `async` script tag in the HTML, and their LCP immediately returned to normal.

### 🤔 **Reflective Questions**

1. What is the difference between `<script async>` and `<script defer>`? Why is `async` generally a good choice for an independent RUM script?
2. Some RUM providers offer to "inline" a tiny loader script directly into the HTML `<head>`. What is the performance benefit of this technique?
3. How can a Content Security Policy (CSP) interfere with your RUM script, especially when you are sending data to a third-party domain?

### 📚 **Further Reading**

- **MDN:** [`<script>`: The Script element (async/defer)](https://www.google.com/search?q=https://developer.mozilla.org/en-US/docs/Web/HTML/Element/script%23async)
- **MDN:** [`requestIdleCallback()`](https://developer.mozilla.org/en-US/docs/Web/API/Window/requestIdleCallback)
- **web.dev:** [Efficiently load third-party JavaScript](https://web.dev/efficiently-load-third-party-javascript/) (The principles apply equally to your own RUM script).

### 📝 **Mini Task (Production)**

- **Your Mission:** Refactor your current RUM implementation to be non-blocking.
    1. If you've been importing `web-vitals` directly into a component, remove that import.
    2. Create a new file in your `public` directory called `vitals-collector.js`.
    3. Place all your `web-vitals` import and callback logic into this new file.
    4. In your main `index.html` file, add a script tag just before the closing `</body>` tag to load this new file asynchronously: `<script async src="/vitals-collector.js"></script>`.
    5. Verify that your application still works and that performance metrics are still being collected and sent to your endpoint. You have now successfully decoupled your RUM script from your main application bundle.