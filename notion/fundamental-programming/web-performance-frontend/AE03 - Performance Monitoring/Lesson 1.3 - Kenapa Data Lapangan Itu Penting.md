### **💡 AE01-1.3: Why Field Data Is King: Understanding Distributions**

**Outline:**

- **The Metric & The Bottleneck (Presentation):** The misleading nature of averages and the need to understand the user experience spectrum.
- **The Optimization Technique (Practice):** A hands-on guide to interpreting percentiles (p75).
- **Your Performance Mission (Production):** Time to analyze a sample data set.

### 📘 **Full Lesson Content**

### **Part 1: Presentation - The Metric & The Bottleneck**

- **The Goal:** Our goal is to stop thinking in averages and start thinking in distributions. We will focus on the **75th percentile (p75)** as the primary metric for analyzing field data, because it gives us a much more stable and representative view of the typical user experience.
- **Identifying the Problem:** The bottleneck is that **averages lie**. An average can be easily skewed by a few extreme outliers. A single user with a 30-second LCP (due to a terrible network) can drag up the average for everyone, making performance look worse than it is. Conversely, a flood of users from a fast internal network can drag the average down, hiding real problems experienced by your target audience. You aren't building a site for the "average" user; you are building a site for a wide spectrum of users.

### **Part 2: Practice - The Optimization Technique**

- **The Principle:** We use percentiles to understand the distribution of our data. The **75th percentile (p75)** is the industry standard for Core Web Vitals.
    
- **What does "p75 LCP is 2.4s" mean?** It means that **75% of your users experienced an LCP of 2.4 seconds _or less_**. The remaining 25% had a worse experience.
    
- **Why is this better than an average?** It's resistant to outliers. That one user with a 30-second LCP has almost no effect on the p75 value. It tells a much clearer story about the experience that the _majority_ of your users are having. Google's threshold for a "Good" experience is defined at the 75th percentile. To pass Core Web Vitals, you need at least 75% of your users to be in the "Good" bucket.
    
    **Example Data (LCP times in seconds):**`[1.2, 1.5, 1.8, 1.9, 2.1, 2.3, 2.4, 2.8, 3.5, 25.0]`
    
    - **Average:** (1.2 + ... + 25.0) / 10 = **4.45s** (Looks "Poor")
    - **p75 (7th value in sorted list):** **2.4s** (Looks "Good")
    
    The p75 tells the real story. The average was a lie created by one extreme outlier.
    

### 🧠 **Real-World Case Study: "The Deceptive Average"**

- **The Scenario:** A SaaS company is monitoring their application's Interaction to Next Paint (INP). The engineering manager looks at the dashboard and sees the _average_ INP is 180ms, which is within the "Good" threshold (<200ms). They assume everything is fine.
- **The Problem:** A senior performance engineer isn't convinced. They switch the dashboard view from "average" to "p75". The p75 INP is a shocking 550ms ("Poor").
- **The Discovery:** By digging into the RUM data, they find the cause. The application is used by two main groups: 80% are internal employees on a super-fast corporate network with powerful desktops, who have an INP of ~50ms. The other 20% are external clients, often on slower laptops and standard WiFi, who are experiencing a terrible INP of over 600ms. The huge number of fast internal users was dragging the _average_ down and completely hiding the severe performance issue affecting their paying customers. Focusing on the p75 forced them to see the truth and fix the problem for their most important user segment.

### 🤔 **Reflective Questions**

1. If the p75 gives a good view of the typical experience, what can looking at the p95 or p99 tell you? What kind of problems might those metrics reveal?
2. Why did Google choose the 75th percentile as the threshold for Core Web Vitals, and not the 50th (median) or 90th?
3. How can an A/B test affect your p75 metrics? If you roll out a "faster" variant to 50% of users, what would you expect to see on your RUM dashboard?

### 📚 **Further Reading**

- **web.dev:** [Defining the Core Web Vitals metrics thresholds](https://web.dev/defining-core-web-vitals-thresholds/)
- **Wikipedia:** [Percentile](https://en.wikipedia.org/wiki/Percentile)
- **Datadog:** [The Power of Percentiles](https://www.google.com/search?q=https://www.datadoghq.com/blog/the-power-of-percentiles/)

### 📝 **Mini Task (Production)**

- **Your Mission:** You have collected the following CLS scores from 10 user sessions: `[0.01, 0.45, 0.05, 0.00, 0.08, 0.11, 0.02, 0.04, 0.09, 0.35]`
    1. Calculate the **average** CLS score.
    2. Sort the scores from lowest to highest.
    3. Identify the **p75** CLS score (the 8th value in the sorted list, as there are 10 values).
    4. Based on Google's thresholds (Good: < 0.1, Needs Improvement: 0.1-0.25, Poor: > 0.25), how would the average score make you feel about your site's performance versus the p75 score?