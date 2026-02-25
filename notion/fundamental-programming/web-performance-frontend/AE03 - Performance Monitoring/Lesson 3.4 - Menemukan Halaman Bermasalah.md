### **💡 AE01-3.4: Pinpointing the Pain: From Global Metrics to Page-Level Problems**

**Outline:**

- **The Metric & The Bottleneck (Presentation):** Knowing your site is slow, but not knowing _where_ it's slow.
- **The Optimization Technique (Practice):** A hands-on guide to using the "Top Pages" view in your RUM dashboard.
- **Your Performance Mission (Production):** Time to analyze a list of underperforming pages and prioritize a fix.

### 📘 **Full Lesson Content**

### **Part 1: Presentation - The Metric & The Bottleneck**

- **The Goal:** Our goal is to transition our analysis from site-wide aggregates to specific, actionable page-level data. This is the final step in diagnosing a problem before you can start fixing it.
- **Identifying the Problem:** You've done your segmentation. You know that LCP is poor, specifically for mobile users. But your site has hundreds of pages. Is the problem on the homepage? The product pages? The blog? The bottleneck is that **a site-wide metric doesn't point you to a line of code**. We need to narrow our focus from the whole site down to a specific page template.

### **Part 2: Practice - The Optimization Technique**

- **The Principle:** RUM tools track the URL for every beacon they collect. This allows them to provide a view, often called "Pages," "Routes," or "URL Groups," that lists the most visited pages and their corresponding performance scores.
    
- **The Analytical Workflow:**
    
    1. Start with your global, segmented view (e.g., p75 LCP for mobile users is 4.2s).
    2. Navigate to the "Pages" or "Routes" tab in your RUM dashboard.
    3. Sort the table by the metric you are investigating (in this case, LCP) in descending order.
    4. Look at the top entries. You will immediately see which pages are the worst offenders.
    
    **Example Table:**
    
    |Path|LCP (p75)|Traffic|
    |---|---|---|
    |`/blog/[slug]`|**5.1s**|40%|
    |`/products/[id]`|4.3s|35%|
    |`/`|2.1s|15%|
    |`/about`|1.9s|10%|
    
    From this table, the problem is crystal clear. The blog post template (`/blog/[slug]`) has a terrible LCP and accounts for a huge portion of your traffic. **This is where you must focus your optimization efforts.** You've successfully moved from a vague site-wide problem to a specific, high-impact target.
    

### 🧠 **Real-World Case Study: "The Overlooked Checkout Page"**

- **The Scenario:** An e-commerce site's overall CLS score is in the "Needs Improvement" range. The team has spent weeks optimizing the homepage and product pages, but the score isn't getting much better.
- **The Problem:** They were focusing on the pages with the most traffic, assuming those were the source of the problem.
- **The Discovery:** A new hire looks at the RUM dashboard, goes to the "Pages" view, and sorts by CLS. They are shocked to see that the `/checkout/payment` page has a catastrophic CLS score of 0.8, even though it has less traffic. A third-party payment widget was loading late and shifting the entire layout. Because this page is so critical to the business, fixing this one high-CLS, lower-traffic page had a massive positive impact on user experience and conversions. They were looking in the wrong place until the data pointed them in the right direction.

### 🤔 **Reflective Questions**

1. When you sort pages by a poor metric, you might see two pages at the top: one with a very bad score but low traffic, and another with a moderately bad score but very high traffic. Which one should you fix first? Why?
2. How can you use page-level RUM data to make your lab-based testing with Lighthouse more effective?
3. Many RUM tools group dynamic routes (e.g., `/products/1` and `/products/2`) into a single entry (`/products/[id]`). Why is this grouping essential for meaningful analysis?

### 📚 **Further Reading**

- **Vercel Docs:** [Analytics View: Pages](https://www.google.com/search?q=https://vercel.com/docs/analytics/analytics-views%23pages)
- **Sentry Docs:** [Performance Trends View](https://www.google.com/search?q=https://docs.sentry.io/product/performance/trends/) (Shows how to identify underperforming pages).
- **SpeedCurve:** [Find Your Worst Performing Pages](https://www.google.com/search?q=https://support.speedcurve.com/en/articles/3248386-finding-your-worst-performing-pages-in-rum)

### 📝 **Mini Task (Production)**

- **Your Mission:** You are a performance engineer looking at the following data table from your RUM tool, which shows the worst-performing pages for CLS on mobile devices. | Path | CLS (p75) | Mobile Traffic Share | | :--- | :--- | :--- | | `/account/settings`| 0.45 | 5% | | `/search` | 0.28 | 45% | | `/` | 0.12 | 30% | | `/contact` | 0.05 | 20% |
    
    Based on this data, which page template represents the **highest priority** for your team to fix? Write one sentence explaining your choice, considering both the severity of the problem (the CLS score) and its impact (the traffic share).