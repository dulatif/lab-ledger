### **💡 AE01-3.1: Decoding the Dashboard: Your First Look at Field Data**

**Outline:**

- **The Metric & The Bottleneck (Presentation):** Information overload and not knowing where to start on a RUM dashboard.
- **The Optimization Technique (Practice):** A guided tour of the key components of a typical RUM dashboard.
- **Your Performance Mission (Production):** Time to interpret a dashboard and identify a key problem.

### 📘 **Full Lesson Content**

### **Part 1: Presentation - The Metric & The Bottleneck**

- **The Goal:** Our goal is to confidently read and understand the primary visualizations in a Real User Monitoring dashboard. We need to translate charts and numbers into a coherent story about our users' experience.
- **Identifying the Problem:** You've enabled RUM, and now you're faced with a screen full of graphs, scores, and tables. The bottleneck is **information overload**. Where do you look first? What do these numbers actually mean? A dashboard without understanding is just noise; our job is to turn it into a signal.

### **Part 2: Practice - The Optimization Technique**

- **The Principle:** Most RUM dashboards (like Vercel's, Sentry's, or Datadog's) share a common structure designed to answer three key questions: How are we doing now? How have we been doing over time? Where are the problems?
- **Key Components to Master:**
    1. **The Scorecard:** This is your at-a-glance summary. It shows your current p75 Core Web Vitals scores (LCP, INP, CLS), usually with a color code (Green for "Good", Orange for "Needs Improvement", Red for "Poor"). **This is where you look first.**
    2. **The Timeline Graph:** This shows how your p75 scores have trended over a period (e.g., the last 24 hours or 7 days). It's crucial for identifying regressions. Did LCP suddenly get worse after yesterday's deployment? This graph will tell you.
    3. **The Distribution Histogram:** This bar chart shows you the full spectrum of user experiences. You can see what percentage of users are in the "Good," "Needs Improvement," and "Poor" buckets for each metric. It provides context for your p75 score.
    4. **The Breakdowns/Filters:** We'll cover this more in later lessons, but this is where you can start slicing your data by page, country, device, etc.

### 🧠 **Real-World Case Study: "The Red Spike"**

- **The Scenario:** A team at a news organization performs a deployment on Tuesday at 4 PM. They check their Lighthouse scores in CI, and everything looks great.
- **The Problem:** On Wednesday morning, the lead engineer opens the RUM dashboard. The Scorecard shows that CLS has moved from "Good" (0.08) to "Poor" (0.28).
- **The Analysis:** They look at the **Timeline Graph**. It shows a flat, green line for CLS for the past week, and then a dramatic, sharp spike into the red zone starting at exactly Tuesday, 4 PM. The graph proves the regression was directly caused by the latest deployment. They didn't need to guess; the data told them exactly when the problem started, allowing them to immediately investigate the code changes from that specific deploy.

### 🤔 **Reflective Questions**

1. Why is a timeline graph more useful for spotting regressions than just looking at the current score?
2. If your p75 LCP is "Good," but the distribution histogram shows that 15% of users are still in the "Poor" category, is your work done? Why or why not?
3. How can a dashboard help you communicate performance issues to non-technical stakeholders like product managers?

### 📚 **Further Reading**

- **Vercel Docs:** [Interpreting the Analytics dashboard](https://www.google.com/search?q=https://vercel.com/docs/analytics/interpreting-the-analytics-dashboard)
- **Sentry Docs:** [Web Vitals Dashboards](https://www.google.com/search?q=https://docs.sentry.io/platforms/javascript/performance/web-vitals/%23dashboards)
- **Datadog:** [Monitoring Core Web Vitals](https://www.google.com/search?q=https://www.datadoghq.com/blog/monitoring-core-web-vitals/)

### 📝 **Mini Task (Production)**

- **Your Mission:** You are given the RUM dashboard for your company's website. You see the following:
    - **LCP Scorecard:** 2.4s (Good)
    - **INP Scorecard:** 180ms (Good)
    - **CLS Scorecard:** 0.29 (Poor)
    - **Timeline Graph:** Shows CLS was stable at ~0.05 until about 24 hours ago, when it jumped to ~0.3. Based _only_ on this information, what is the single most important piece of information you would report to your team? What is your immediate hypothesis for the cause?