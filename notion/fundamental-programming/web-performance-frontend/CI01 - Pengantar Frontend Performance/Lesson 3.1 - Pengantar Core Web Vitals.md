💡 CI01-3.1: The Three Pillars of User Experience

Outline:

- **The Metric & The Bottleneck (Presentation):** Introducing the "why" behind Core Web Vitals.
- **The Optimization Technique (Practice):** Defining the three core metrics: LCP, INP, and CLS.
- **Your Performance Mission (Production):** Classifying real-world user frustrations into Core Web Vitals.

📘 **Full Lesson Content**

### **Part 1: Presentation - The Metric & The Bottleneck**

- **The Goal:** Today, we're moving from a general performance score to the three specific metrics that Google has deemed most critical for user experience: the **Core Web Vitals (CWV)**. These are the foundational metrics that directly influence your SEO page experience ranking.
- **Identifying the Problem:** The bottleneck is noise. A Lighthouse report gives you a dozen different metrics. Which ones _really_ matter? By focusing on CWV, we filter out the noise and concentrate on what users truly feel. Google's research simplified user experience into three core questions a user implicitly asks:
    1. Is it loading? (**Loading performance**)
    2. Can I interact with it? (**Interactivity**)
    3. Is it visually stable? (**Visual stability**)

### **Part 2: Practice - The Optimization Technique**

- **The Principle:** The technique is to learn the "language" of Core Web Vitals. Each vital answers one of the core user questions with a specific, measurable metric.
    - **1. LCP (Largest Contentful Paint) - Measures _Loading_**
        - **User Question:** "Is this page useful yet?"
        - **What it is:** The time it takes for the largest image or text block visible within the viewport to render. It's a proxy for when the main content of the page has likely loaded.
        - **Goal:** Below 2.5 seconds.
    - **2. INP (Interaction to Next Paint) - Measures _Interactivity_**
        - **User Question:** "Did the page respond to my click?"
        - **What it is:** Measures the latency of all user interactions (clicks, taps, key presses) with a page. A high INP means the page feels sluggish or unresponsive. It replaced FID (First Input Delay).
        - **Goal:** Below 200 milliseconds.
    - **3. CLS (Cumulative Layout Shift) - Measures _Visual Stability_**
        - **User Question:** "Did that thing I was about to click just move?"
        - **What it is:** Measures the sum total of all unexpected layout shifts that occur. A high CLS score means the page is janky and frustrating to use.
        - **Goal:** Below 0.1.

### 🧠 **Real-World Case Study: "The Core Vitals Dashboard"**

- **The Scenario:** A large e-commerce company wants to create a single, high-level dashboard for executives to monitor the health of their website's user experience.
- **The Implementation:** Instead of showing dozens of technical metrics, the engineering team builds the dashboard around just three graphs: LCP, INP, and CLS, pulled from their real-user monitoring (RUM) data. Each graph shows the percentage of users in the "Good," "Needs Improvement," and "Poor" buckets.
- **The Takeaway:** This dashboard became the company's "source of truth" for user experience. When a new code release caused CLS to spike, the executives could see the impact immediately without needing to understand the technical cause. It aligned the entire company—from engineers to the C-suite—around a shared, simple, and powerful set of metrics.

### 🤔 **Reflective Questions**

1. Why do you think Google chose to elevate these three metrics above all others like FCP (First Contentful Paint) or TBT (Total Blocking Time)?
2. Can a page have a good LCP but a poor INP? What would that experience feel like for a user?
3. Can a page have a good INP but a poor CLS? What would that experience feel like?

### 📚 **Further Reading**

- **Web Vitals:** [The official introduction to Core Web Vitals](https://web.dev/articles/vitals)
- **INP Explained:** [Learn all about the newest Core Web Vital, INP](https://web.dev/articles/inp)
- **Auditing Tool:** [PageSpeed Insights](https://pagespeed.web.dev/) (The best place to see your site's CWV scores).

### 📝 **Mini Task (Production)**

- **Your Mission:** Think about the last three frustrating experiences you had on a website.
- **Your Deliverable:** For each frustration, write one sentence describing it and then classify it as a likely LCP, INP, or CLS issue.
    - _Example 1:_ "I loaded a news article, and just as I went to click the headline, a giant ad loaded at the top and pushed it down. That's a **CLS** issue."
    - _Example 2:_ "I was on a checkout page and clicked the 'Pay Now' button, but nothing happened for a second. The page felt frozen. That's an **INP** issue."
    - _Example 3:_ "I visited a landing page, and for a few seconds, all I saw was the header and a blank white space where the main image should have been. That's an **LCP** issue."