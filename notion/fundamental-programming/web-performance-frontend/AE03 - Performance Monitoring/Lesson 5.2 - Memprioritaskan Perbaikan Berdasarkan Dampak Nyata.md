### **💡 AE01-5.2: Prioritizing Fixes Based on Real, User-Centric Impact**

**Outline:**

- **The Metric & The Bottleneck (Presentation):** An endless backlog of bugs and performance tickets with no clear way to decide what to work on next.
- **The Optimization Technique (Practice):** Using RUM and error data to create an Impact vs. Effort matrix for prioritization.
- **Your Performance Mission (Production):** Time to prioritize a list of issues based on real-world data.

### 📘 **Full Lesson Content**

### **Part 1: Presentation - The Metric & The Bottleneck**

- **The Goal:** Our objective is to move away from prioritizing work based on gut feelings or "who shouts loudest" and toward a data-driven framework. We need to focus our limited engineering time on the issues that have the biggest negative impact on the most users.
- **Identifying the Problem:** Your team's backlog has 200 tickets. One is a bug that affects 10,000 users a day. Another is a performance issue that gives 500 high-value enterprise users a terrible experience. Which one is more important? The bottleneck is a **lack of a prioritization framework**. Without one, teams often work on what seems easiest or what a senior manager complained about, not what's most impactful.

### **Part 2: Practice - The Optimization Technique**

- **The Principle:** We will use data from our monitoring tools to quantify the **Impact** of each issue. Impact is a function of severity (how bad is the problem?) and scale (how many users does it affect?). We then weigh this against the estimated **Effort** to fix it.
- **The Impact vs. Effort Matrix:**
    - **High-Impact, Low-Effort (Quick Wins):** Your #1 priority. These are the fixes that give you the most bang for your buck.
    - **High-Impact, High-Effort (Major Projects):** Important strategic initiatives that need to be planned.
    - **Low-Impact, Low-Effort (Fill-in Tasks):** Good to fix when you have spare time.
    - **Low-Impact, High-Effort (The Time Sinks):** Avoid these unless they are blocking other work.
- **Quantifying Impact with Data:**
    - **For Errors:** How many users are affected per hour? Sentry tells you this directly. An error affecting 1,000 users/hour is higher impact than one affecting 10.
    - **For Performance Issues:** What is the p75 score, and what percentage of traffic does this page receive? A page with a 5s LCP that gets 40% of your traffic is higher impact than a page with a 6s LCP that gets 2% of traffic.

### 🧠 **Real-World Case Study: "The 'Annoying' vs. The 'Expensive' Bug"**

- **The Scenario:** A SaaS company has two major issues in their backlog.
    - **Issue A:** A CSS bug makes a button misaligned by 2px in the user settings page. The lead designer is very vocal about fixing it.
    - **Issue B:** A Sentry error shows that 5% of users are experiencing a silent failure when trying to use the "Export Data" feature, a key function for paying customers.
- **The Prioritization:** The junior PM wants to fix Issue A because it's visually jarring and the designer is complaining. The senior engineer pulls up the data.
    - **Issue A (CSS Bug):** _Impact:_ Low. It's a cosmetic issue on a low-traffic page. _Effort:_ Low.
    - **Issue B (Export Error):** _Impact:_ High. It's breaking a core, paid feature for a significant number of users. _Effort:_ Medium.
- **The Decision:** By framing the decision with data, the team agrees to prioritize Issue B. It's a higher-impact problem directly affecting revenue. Issue A is categorized as a "fill-in task." Data removed the emotion and subjectivity from the decision.

### 🤔 **Reflective Questions**

1. How can you factor business goals into your impact assessment? (e.g., an error on the checkout page is more impactful than one on the "About Us" page).
2. Who should be involved in the prioritization process? Is it just an engineering decision?
3. What are the risks of _only_ working on high-impact, low-effort "quick wins"?

### 📚 **Further Reading**

- **Sentry Blog:** [How to Triage and Prioritize Errors](https://www.google.com/search?q=https://sentry.io/blog/triage-prioritize-errors/)
- **Asana:** [Learn about the action priority matrix](https://www.google.com/search?q=https://asana.com/resources/action-priority-matrix)
- **Nielsen Norman Group:** [Severity Ratings for Usability Problems](https://www.nngroup.com/articles/how-to-rate-the-severity-of-usability-problems/) (A similar framework for UX issues).

### 📝 **Mini Task (Production)**

- **Your Mission:** You are a team lead and need to decide which of the following three issues to prioritize for the next sprint.
    
    1. **Issue A:** A `TypeError` in Sentry affects 2% of users on the marketing homepage. (Effort: Low)
    2. **Issue B:** The p75 LCP of the `/pricing` page, which gets 30% of all traffic, is 4.5s. (Effort: High)
    3. **Issue C:** A 404 error is being logged for a missing image on the `/about` page, which gets 1% of traffic. (Effort: Low)
    
    Which issue is the highest priority **Quick Win**? Which is the most important **Major Project**? Explain your reasoning in one sentence for each.