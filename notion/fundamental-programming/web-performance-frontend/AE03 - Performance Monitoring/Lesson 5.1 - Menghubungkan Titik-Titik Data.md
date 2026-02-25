### **💡 AE01-5.1: Connecting the Dots: When Errors Cause Poor Performance**

**Outline:**

- **The Metric & The Bottleneck (Presentation):** Data silos; having performance data in one dashboard and error data in another.
- **The Optimization Technique (Practice):** Correlating spikes in errors with dips in performance metrics to find root causes.
- **Your Performance Mission (Production):** Time to play detective with data from two different sources.

### 📘 **Full Lesson Content**

### **Part 1: Presentation - The Metric & The Bottleneck**

- **The Goal:** Our objective is to develop a holistic view of application health by combining insights from both Real User Monitoring (RUM) and Error Tracking. We need to see them not as separate tools, but as two sides of the same coin.
- **Identifying the Problem:** You have your Vercel Analytics dashboard open showing a sudden drop in your LCP score, and a separate Sentry dashboard showing a spike in a new JavaScript error. The bottleneck is a **cognitive disconnect**. You are forced to manually switch between tools and try to guess if the two events are related. This slows down debugging and makes it harder to see the full picture.

### **Part 2: Practice - The Optimization Technique**

- **The Principle:** A significant portion of performance problems are either caused or exacerbated by client-side errors. A JavaScript error that blocks the main thread can halt rendering, leading to a terrible LCP or INP. A failed API call for an image can cause a massive layout shift (CLS) when the error message appears. The key is to look for **correlation on a timeline**.
    
- **The Unified View:** Modern observability platforms like Sentry are now integrating RUM and error tracking into a single interface.
    
    **How to Connect the Dots Manually:**
    
    1. **Spot a Regression:** In your RUM tool, identify a performance metric that got worse at a specific time (e.g., "LCP dropped from 2.1s to 4.5s on Tuesday at 2:15 PM").
    2. **Pivot to Errors:** Go to your error tracking tool. Filter for errors that started appearing for the first time, or dramatically increased in volume, at that _exact same time_.
    3. **Form a Hypothesis:** If you find a matching error (e.g., a `TypeError` in your image rendering component started spiking at 2:15 PM), you have a prime suspect. The error is likely blocking the rendering of your LCP element.

### 🧠 **Real-World Case Study: "The INP Killer"**

- **The Scenario:** A social media site's RUM dashboard shows that their p75 Interaction to Next Paint (INP) has suddenly degraded from a healthy 150ms to a sluggish 500ms. Users are complaining that clicking the "like" button feels laggy.
- **The Problem:** The team inspects the "like" button component code, but can't see any obvious performance bottlenecks.
- **The Connection:** An engineer pulls up Sentry and looks at the timeline. At the exact moment the INP started to degrade, a new, seemingly unrelated error started spiking: `SecurityError: Blocked a frame with origin "..." from accessing a cross-origin frame`. The error was coming from an embedded third-party analytics script. This error was firing on every user interaction, and even though it was being caught, the sheer volume was creating enough main-thread contention to delay the rendering of the "like" button's visual feedback, thus killing their INP score.

### 🤔 **Reflective Questions**

1. What kind of error would you suspect if you saw a correlation between its appearance and a sudden increase in CLS?
2. Why is it valuable to have both RUM and error tracking data available to Product Managers, not just engineers?
3. Besides errors, what other events could you correlate with performance data to find insights? (e.g., deployment markers, feature flag changes).

### 📚 **Further Reading**

- **Sentry Blog:** [Connecting the Dots Between Application Errors and Performance](https://www.google.com/search?q=https://sentry.io/blog/connecting-application-errors-and-performance-monitoring/)
- **Datadog:** [Correlate frontend errors with RUM sessions](https://docs.datadoghq.com/real_user_monitoring/error_tracking/explorer/)
- **Smashing Magazine:** [A Guide To Getting The Most Out Of Your Error-Tracking Tool](https://www.google.com/search?q=https://www.smashingmagazine.com/2021/08/guide-error-tracking-tool/)

### 📝 **Mini Task (Production)**

- **Your Mission:** You are a performance engineer presented with two pieces of data from Tuesday morning.
    
    - **RUM Data:** The p75 LCP for your e-commerce site's product pages (`/products/[id]`) jumped from 2.4s to 6.0s.
    - **Error Data:** A new Sentry issue appeared: `TypeError: Cannot read properties of undefined (reading 'url')` in your `ProductImageGallery.jsx` component. This issue is only happening on product pages.
    
    Write a one-sentence hypothesis that connects these two data points and explains the likely cause of the LCP regression.