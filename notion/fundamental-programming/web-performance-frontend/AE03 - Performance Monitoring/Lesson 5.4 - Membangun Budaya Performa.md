### **💡 AE01-5.4: From Lone Wolf to Wolf Pack: Building a Performance Culture**

**Outline:**

- **The Metric & The Bottleneck (Presentation):** Performance is seen as one person's job, leading to burnout and inconsistent results.
- **The Optimization Technique (Practice):** Actionable steps to make performance a shared responsibility for the entire team.
- **Your Performance Mission (Production):** Time to craft a message to your team to kickstart a performance-aware culture.

### 📘 **Full Lesson Content**

### **Part 1: Presentation - The Metric & The Bottleneck**

- **The Goal:** Our final and most important goal is to embed performance into our team's DNA. Performance shouldn't be a heroic, one-off effort by a single expert; it should be a consistent, team-wide consideration, just like code quality or accessibility.
- **Identifying the Problem:** One person becomes the "performance police." They are the only one looking at the dashboards and filing tickets. When they go on vacation, performance regresses. The bottleneck is a **lack of shared ownership**. When only one person cares, performance will always be a secondary concern, tacked on at the end of a project instead of being built in from the start.

### **Part 2: Practice - The Optimization Technique**

- **The Principle:** Building a performance culture is about making performance visible, understood, and valued by everyone—engineers, product managers, designers, and leadership. It's a social and technical challenge.
- **Actionable Steps to Build the Culture:**
    1. **Make Data Visible:**
        - Put your RUM and Sentry dashboards on a shared screen in the office or a pinned tab in your team's Slack channel.
        - Set up automated alerts for significant regressions that notify the whole team, not just one person.
    2. **Define a Shared Language (Performance Budgets):**
        - Agree on clear, objective goals. Example: "No page's p75 LCP should ever be over 2.5 seconds." or "Our main JavaScript bundle size must not exceed 150kB."
        - Enforce these budgets automatically in your CI/CD pipeline using tools like Lighthouse CI. A pull request that violates the budget should fail its checks.
    3. **Integrate into Your Rituals:**
        - **Sprint Planning:** When discussing a new feature, ask the question: "What is the performance impact of this?"
        - **Retrospectives:** Dedicate 5 minutes to review the performance dashboard. Did we improve or regress this sprint?
        - **Demos:** Encourage engineers to show the Lighthouse score or Profiler recording for their new feature, not just the feature itself.
    4. **Celebrate Wins:**
        - When a team deploys a fix that measurably improves a Core Web Vital, celebrate it publicly. Show the "before" and "after" graphs from the RUM dashboard. This reinforces that the work is valued.

### 🧠 **Real-World Case Study: "The Slack Alert"**

- **The Old Way:** A performance engineer at a media company would manually check the RUM dashboard every morning. If they saw a regression, they would have to find the right team, explain the problem, and convince them to prioritize a fix. It was an uphill battle.
- **The New Way:** They configured an automated alert. If the p75 CLS for any page went above 0.1, a message with a link to the dashboard was automatically posted in the relevant team's public Slack channel.
- **The Culture Shift:** The very next day, a deployment caused a CLS regression. The alert fired. The entire team saw it immediately. The engineer who wrote the code said, "Oh, that's my change," and had a fix deployed within an hour. By making the data visible and the feedback loop immediate, performance became a self-correcting, team-wide responsibility.

### 🤔 **Reflective Questions**

1. How can a designer's choices (e.g., using a large, unoptimized video in the hero section) impact performance? How can you involve them in the performance culture?
2. What is the role of a Product Manager in a performance-aware team? How do they balance feature velocity with performance goals?
3. "What gets measured gets managed." How does this phrase relate to building a performance culture?

### 📚 **Further Reading**

- **Google I/O:** [Building a performance culture](https://www.google.com/search?q=https://www.youtube.com/watch%3Fv%3D_S9sP9_I65A) (A great talk on this topic).
- **web.dev:** [Establish a performance culture](https://www.google.com/search?q=https://web.dev/establish-a-performance-culture/)
- **Lighthouse CI:** [Getting Started with Lighthouse CI](https://web.dev/lighthouse-ci/) (A key tool for performance budgets).

### 📝 **Mini Task (Production)**

- **Your Mission:** You are the tech lead of a team that has just successfully integrated RUM and error tracking. You now need to introduce these tools to the team and set the tone for your new performance culture.
    
    Write a short, encouraging message (3-5 sentences) to post in your team's Slack channel. Your message should:
    
    1. Announce that the new monitoring tools are live.
    2. Briefly explain why this is important for your users.
    3. Emphasize that this is a shared responsibility.
    4. Include a link to the shared dashboard.