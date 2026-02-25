### **💡 AE01-2.2: Going Manual: Collecting Vitals with `web-vitals`**

**Outline:**

- **The Metric & The Bottleneck (Presentation):** The limitations and lack of control in platform-integrated RUM.
- **The Optimization Technique (Practice):** A hands-on guide to using Google's `web-vitals` library to collect metrics.
- **Your Performance Mission (Production):** Time to install the library and log vitals to the console.

### 📘 **Full Lesson Content**

### **Part 1: Presentation - The Metric & The Bottleneck**

- **The Goal:** Our goal is to gain direct, code-level access to the Core Web Vitals metrics as they are measured in the user's browser. This gives us the ultimate flexibility to use the data however we see fit.
- **Identifying the Problem:** Integrated analytics (like Vercel's) are fantastic for getting started, but they are a black box. You can't add your own custom data, you can't send the metrics to a different analytics provider, and you can't inspect the raw data easily. The bottleneck is a **lack of control**. To do more advanced performance analysis, we need to own the data collection process ourselves.

### **Part 2: Practice - The Optimization Technique**

- **The Principle:** Google provides a tiny, open-source JavaScript library called `web-vitals`. Its sole purpose is to provide a simple, robust API for measuring the Core Web Vitals, exactly as Google measures them. It handles all the complex edge cases so you don't have to.
    
- **Secure Code Implementation:Step 1: Installation**
    
    ```
    npm install web-vitals
    
    ```
    
    **Step 2: Implementation** In your application's main entry point (e.g., `_app.tsx` in Next.js, or `index.js` in Create React App), you import the reporting functions and pass them a callback.
    
    ```tsx
    import { onLCP, onINP, onCLS } from 'web-vitals';
    
    // This function will be called by the library when a metric is ready.
    function logVital(metric) {
      console.log(metric);
    }
    
    // Register the callbacks
    onLCP(logVital);
    onINP(logVital);
    onCLS(logVital);
    
    ```
    
    Now, when you run your application and interact with it, you will see objects being logged to the console. Each object represents a measured metric and contains valuable data like its `name`, `value`, `rating` (good/needs improvement/poor), and a unique `id`.
    
    **Example Console Output for CLS:**
    
    ```ts
    {
      name: 'CLS',
      value: 0.0871,
      rating: 'good',
      id: 'v3-1662557765325-224927138098',
      // ... more properties
    }
    
    ```
    

### 🧠 **Real-World Case Study: "Correlating Vitals with A/B Tests"**

- **The Scenario:** An e-commerce company is running an A/B test on their product page. Variant "A" is the original design, and Variant "B" uses a new, interactive image gallery.
    
- **The Problem:** Their platform-integrated RUM shows that overall LCP is getting slightly worse, but they can't tell if it's because of the A/B test or some other factor.
    
- **The Solution:** They implement the `web-vitals` library. In their callback function, before sending the data, they check which A/B test variant the user is seeing and add that information to the data payload.
    
    ```ts
    function sendToAnalytics(metric) {
      const variant = getABTestVariant(); // Function to get 'A' or 'B'
      const data = {
        name: metric.name,
        value: metric.value,
        variant: variant
      };
      // Send `data` to their analytics backend...
    }
    onLCP(sendToAnalytics);
    
    ```
    
    Now, in their analytics platform, they can segment their LCP scores by `variant`. They quickly discover that Variant "B" has a significantly worse LCP. The `web-vitals` library gave them the control they needed to precisely measure the performance impact of a business decision.
    

### 🤔 **Reflective Questions**

1. Why is a dedicated library like `web-vitals` better than trying to use the browser's `PerformanceObserver` API directly to measure these metrics?
2. The `onINP` and `onCLS` callbacks can fire multiple times for a single page view. Why? How does this reflect the nature of these particular metrics?
3. The `web-vitals` library also reports metrics like TTFB (Time to First Byte) and FCP (First Contentful Paint). How are these different from the Core Web Vitals?

### 📚 **Further Reading**

- **GitHub:** [GoogleChrome/web-vitals](https://github.com/GoogleChrome/web-vitals) (The official repository is the best documentation).
- **web.dev:** [An introduction to the web-vitals library](https://www.google.com/search?q=https://web.dev/vitals-library/)
- **MDN:** [PerformanceObserver API](https://developer.mozilla.org/en-US/docs/Web/API/PerformanceObserver) (The underlying browser API used by the library).

### 📝 **Mini Task (Production)**

- **Your Mission:** Install the `web-vitals` library in a React/Next.js project and see the raw metrics for yourself.
    1. In your project directory, run `npm install web-vitals`.
    2. In a top-level component file that runs on every page (like `_app.tsx` or `App.js`), import `onLCP`, `onINP`, and `onCLS`.
    3. Call each of these functions, passing a simple callback that `console.log`s the metric it receives.
    4. Run your app, load a page, and interact with it (click around). Watch your browser's developer console to see the metric objects as they are reported.