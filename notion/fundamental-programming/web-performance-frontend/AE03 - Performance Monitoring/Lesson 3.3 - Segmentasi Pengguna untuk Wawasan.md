### **💡 AE01-3.3: Slicing the Data: Finding Patterns with Segmentation**

**Outline:**

- **The Metric & The Bottleneck (Presentation):** A single site-wide metric hides the "who" and "why" behind performance issues.
- **The Optimization Technique (Practice):** A hands-on guide to filtering RUM data by key dimensions.
- **Your Performance Mission (Production):** Time to devise a segmentation strategy to solve a performance mystery.

### 📘 **Full Lesson Content**

### **Part 1: Presentation - The Metric & The Bottleneck**

- **The Goal:** Our goal is to move beyond aggregate metrics and learn to segment our RUM data. Segmentation allows us to isolate performance problems to specific user groups, revealing patterns that are invisible at the macro level.
- **Identifying the Problem:** Your dashboard shows a poor p75 CLS score of 0.3. This is a critical piece of information, but it's not actionable. You know the "what" (CLS is bad), but you don't know the "who," "where," or "how." The bottleneck is that a **global metric lacks context**. Without context, you're just guessing where to start looking for a fix.

### **Part 2: Practice - The Optimization Technique**

- **The Principle:** RUM tools collect valuable metadata with every performance beacon. We can use this metadata to filter and group our data, a process called segmentation. This is the primary method for turning a metric into an insight.
- **The Most Powerful Segments:**
    1. **Device Type:** `Desktop` vs. `Mobile` vs. `Tablet`. This is often the most revealing segment. Is your LCP problem only happening on mobile?
    2. **Country:** Are users in a specific geographic region having a worse experience due to network latency to your servers?
    3. **Connection Type (Effective):** `4G` vs. `3G` vs. `slow-2g`. This helps you understand how your site performs under realistic network conditions.
    4. **Browser:** Is a performance issue specific to Safari on iOS?
    5. **Page/Route:** (We'll cover this next lesson) Which specific pages are the worst offenders?
- **The Workflow:**
    1. Start with the global p75 score.
    2. If a metric is poor, form a hypothesis. "I bet this is a mobile problem."
    3. Apply the `Device Type: Mobile` filter.
    4. Observe the new p75 score for just that segment. If it's much worse than the global score, your hypothesis is likely correct.

### 🧠 **Real-World Case Study: "The German LCP Problem"**

- **The Scenario:** A US-based e-commerce company launches a marketing campaign in Germany. Their global p75 LCP is a healthy 2.2s.
- **The Problem:** The marketing team reports that the campaign in Germany is underperforming, and they've received feedback that the site is slow for German users. The global LCP score doesn't show a problem.
- **The Analysis:** A performance engineer opens the RUM dashboard and applies a filter: `Country: Germany`. The view updates, and the p75 LCP for the German segment is a dismal 4.8s. They've found the smoking gun. By segmenting the data, they confirmed a real user problem that was completely hidden by the global metric. The cause turned out to be an unoptimized, high-resolution hero image that was taking forever to download over the transatlantic connection. They implemented a Germany-specific CDN and properly sized images, and the LCP for that segment dropped dramatically.

### 🤔 **Reflective Questions**

1. Why is segmenting by `Device Type` often the first and most important step in diagnosing a performance issue?
2. You see that users on `3G` have a much worse LCP than users on `4G`. Is this automatically a problem you need to fix? Or is it expected? How do you decide?
3. What custom metadata could you add to your RUM beacons to create even more powerful segments specific to your business? (e.g., `user_type: guest` vs `user_type: pro`).

### 📚 **Further Reading**

- **Vercel Docs:** [Filtering the Analytics dashboard](https://www.google.com/search?q=https://vercel.com/docs/analytics/filtering-the-analytics-dashboard)
- **Akamai:** [The Importance of Data Segmentation in RUM](https://www.google.com/search?q=https://www.akamai.com/blog/performance/the-importance-of-data-segmentation-in-rum)
- **Sentry Docs:** [Custom Tags for Filtering](https://www.google.com/search?q=https://docs.sentry.io/platforms/javascript/performance/instrumentation/custom-instrumentation/%23custom-tags) (Shows how to add custom metadata for segmentation).

### 📝 **Mini Task (Production)**

- **Your Mission:** You are a performance engineer at a SaaS company. Your global p75 INP is 210ms ("Needs Improvement"). You need to devise a plan to find the root cause. List the **top three segments** you would investigate, in order. For each segment, state what question you are trying to answer by applying that filter.
    - Example: `1. Segment: Device Type. Question: Is the poor INP specific to mobile users, who might have less processing power?`