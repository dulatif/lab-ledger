### **💡 AE01-3.2: Why Averages Lie: Mastering the 75th Percentile**

**Outline:**

- **The Metric & The Bottleneck (Presentation):** The misleading nature of averages and how they hide real problems.
- **The Optimization Technique (Practice):** A practical guide to using p75 as your source of truth for field data.
- **Your Performance Mission (Production):** Time to analyze a raw data set and see the difference for yourself.

### 📘 **Full Lesson Content**

### **Part 1: Presentation - The Metric & The Bottleneck**

- **The Goal:** This lesson reinforces a critical concept: **stop looking at averages**. Our goal is to cement the **75th percentile (p75)** as the standard unit of measurement for analyzing user experience in the field.
- **Identifying the Problem:** We covered this conceptually in Module 1, but now we're putting it into practice. The bottleneck in analysis is clinging to familiar but flawed metrics. Using the "average" to measure performance is like measuring a river's depth by its average; it won't tell you about the 1-foot shallows or the 20-foot deep hole. It hides the extremes of user experience and can lead to a false sense of security.

### **Part 2: Practice - The Optimization Technique**

- **The Principle:** The p75 is the value for which 75% of your users have an experience that is _that good or better_. It's the standard chosen by Google for passing Core Web Vitals because it's a much more stable and representative measure of the "typical" user experience than the average. It intentionally ignores the worst outliers to prevent noise, while still capturing the experience of a vast majority of users.
    
- **How to Analyze with p75:**
    
    1. **Set Your Dashboard:** The first thing you should do in any RUM tool is find the toggle to switch from "Average" to "p75". Make p75 your default view.
    2. **Compare and Contrast:** Let's look at a real data set of LCP values from 12 sessions: `[1.8, 1.9, 2.0, 2.1, 2.2, 2.3, 2.4, 2.4, 2.5, 2.8, 4.5, 20.0]`
        - **Average Calculation:** `(Sum of all values) / 12 = 49.9 / 12 = 4.16s`
            - **Conclusion from Average:** Our LCP is "Poor". We need to panic!
        - **p75 Calculation:** Sort the data and find the value at the 75% mark (the 9th value).
            - Sorted: `[1.8, 1.9, 2.0, 2.1, 2.2, 2.3, 2.4, 2.4, **2.5**, 2.8, 4.5, 20.0]`
            - **Conclusion from p75:** Our LCP is 2.5s, which is right on the cusp of "Good". The experience is acceptable for the vast majority of users, but we have a long-tail problem with a few very slow sessions.
    
    The p75 gives us a much more actionable and less alarmist signal. It tells us our baseline is okay, but we should investigate the outliers. The average just told us to panic.
    

### 🧠 **Real-World Case Study: "The Bot Traffic Illusion"**

- **The Scenario:** An engineering team is proud of their website's performance. Their RUM dashboard, which defaults to showing the _average_ LCP, reports a blazing-fast 1.2s.
- **The Problem:** Despite the great average, they keep getting anecdotal feedback from the marketing team that "the site feels slow."
- **The Investigation:** A performance-focused engineer switches the dashboard view to p75. The p75 LCP is 3.8s ("Needs Improvement"). How could the numbers be so different? They discover that their site is being constantly crawled by monitoring bots running on high-speed servers. These bots have a sub-second LCP. This massive volume of non-human traffic was dragging the _average_ LCP down significantly, creating an illusion of speed and completely hiding the sluggish experience that their real, human users were facing. Switching to p75 instantly revealed the truth.

### 🤔 **Reflective Questions**

1. If your p75 LCP is good, but your p95 LCP is terrible, what might that indicate about your user base or application?
2. Can the average ever be a useful metric in performance analysis? If so, in what specific scenarios?
3. How does focusing on the p75 align with Google's goal of improving the web for the majority of users?

### 📚 **Further Reading**

- **web.dev:** [Defining the Core Web Vitals metrics thresholds](https://web.dev/defining-core-web-vitals-thresholds/) (The official source on the p75 standard).
- **Wikipedia:** [Percentile](https://en.wikipedia.org/wiki/Percentile)
- **ACPM:** [Why is the 75th Percentile the new Average?](https://www.google.com/search?q=https://www.akamai.com/blog/performance/why-is-the-75th-percentile-the-new-average)

### 📝 **Mini Task (Production)**

- **Your Mission:** You are a performance engineer given the following INP scores (in milliseconds) from 8 user sessions: `[80, 120, 450, 95, 150, 1100, 130, 110]`
    1. Calculate the **average** INP.
    2. Calculate the **p75** INP (the 6th value in the sorted list).
    3. Based on the Core Web Vitals thresholds (Good < 200ms), write one sentence summarizing what the average tells you, and one sentence summarizing what the p75 tells you. Which metric provides a more accurate picture of the typical user's experience?