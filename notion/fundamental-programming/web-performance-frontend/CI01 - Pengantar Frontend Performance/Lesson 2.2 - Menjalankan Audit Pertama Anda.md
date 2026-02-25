💡 CI01-2.2: From Zero to Audit

Outline:

- **The Metric & The Bottleneck (Presentation):** The goal of running a consistent, repeatable performance audit.
- **The Optimization Technique (Practice):** A step-by-step guide to running Lighthouse directly from Chrome DevTools.
- **Your Performance Mission (Production):** Auditing two websites and comparing their scores.

📘 **Full Lesson Content**

### **Part 1: Presentation - The Metric & The Bottleneck**

- **The Goal:** Today's goal is simple but fundamental: to successfully run your first **Lighthouse audit**. The result of this action is our key metric—a full performance report.
- **Identifying the Problem:** The bottleneck is "analysis paralysis" or not knowing where to start. Many developers know they _should_ test performance, but they aren't familiar with the easiest, most accessible entry point. We're going to remove that barrier by using the tool that's already built into the browser you use every day.

### **Part 2: Practice - The Optimization Technique**

- **The Principle:** The most effective technique is the one you'll use consistently. Running Lighthouse from Chrome DevTools is the fastest way to get feedback while you're developing. It allows you to audit a page, make a code change, and immediately re-audit to see the impact.
- **Secure Code Implementation (A Step-by-Step Guide):**
    1. **Open in Incognito Mode:** Open a new Incognito window in Google Chrome. This is crucial because it prevents your installed browser extensions from interfering with the audit and skewing the performance results.
    2. **Navigate to the URL:** Go to the webpage you want to audit. Let it load completely.
    3. **Open Chrome DevTools:** Right-click anywhere on the page and select "Inspect," or use the shortcut (`Ctrl+Shift+I` on Windows/Linux, `Cmd+Option+I` on Mac).
    4. **Find the Lighthouse Tab:** In the DevTools panel, find and click on the "Lighthouse" tab. It might be hidden behind the `>>` icon.
    5. **Configure Your Audit:**
        - **Mode:** Leave it on "Navigation (Default)."
        - **Device:** **Always start with "Mobile."** Most users are on mobile, and performance bottlenecks are more obvious on slower devices.
        - **Categories:** For now, you can leave them all checked.
    6. **Analyze Page Load:** Click the "Analyze page load" button. DevTools will take over, simulate a mobile device, and run the audit. This will take about 30-60 seconds.
    7. **View Your Report:** Once it's done, a full report will appear directly in the Lighthouse panel.

### 🧠 **Real-World Case Study: "The Pre-Commit Check"**

- **The Scenario:** A React engineer is working on a new feature for an e-commerce product page. They've added a new third-party script for customer reviews.
- **The Implementation:** Before committing their code and creating a pull request, the engineer follows the steps above to run a quick Lighthouse audit on their local development environment.
- **The Result:** They immediately see that the Performance score has dropped by 15 points. The new script is render-blocking and has significantly worsened the Total Blocking Time (TBT).
- **The Takeaway:** By running a 60-second audit, the engineer caught a major performance regression _before_ it ever reached the code review stage. This habit of "auditing before you commit" saves the entire team time and prevents performance issues from ever making it to production.

### 🤔 **Reflective Questions**

1. Why is it critical to run audits in an Incognito window and with a "Mobile" device selected? What kind of skewed results might you get if you don't?
2. Lighthouse is available in DevTools, as a standalone CLI tool, and in services like PageSpeed Insights. When would the DevTools approach be most useful? When might you prefer an automated tool?
3. What do you think happens to the audit results if you run them on a very slow Wi-Fi connection versus a very fast one? How does this relate to the concept of "lab data"?

### 📚 **Further Reading**

- **Web Vitals:** [An introduction to Core Web Vitals on web.dev](https://web.dev/articles/vitals)
- **Official Guide:** [Lighthouse in Chrome DevTools](https://www.google.com/search?q=https://developer.chrome.com/docs/lighthouse/run/devtools)
- **Auditing Tool:** You're learning it right now! [Google Lighthouse](https://developer.chrome.com/docs/lighthouse/)

### 📝 **Mini Task (Production)**

- **Your Mission:** Time to get your hands dirty.
    1. Pick your favorite, high-traffic website (e.g., a news site, a social media platform, an e-commerce store).
    2. Pick one of its direct competitors.
- **Your Deliverable:** Run a **mobile** Lighthouse audit on the homepage of both websites. Take a screenshot of the top of the report showing the five scores for each site. In one sentence, declare which site has the better overall performance score.