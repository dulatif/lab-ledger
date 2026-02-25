# 💡 AM01.1: Your Performance Compass: Auditing with Lighthouse

**Outline:**

- **You Can't Fix What You Can't Measure (Presentation):** Introducing Lighthouse as your essential diagnostic tool.
- **Running Your First Audit (Practice):** A hands-on guide to generating and reading a Lighthouse report.
- **From Numbers to Action (Production):** Applying Lighthouse's recommendations to a real-world problem.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - You Can't Fix What You Can't Measure**

Welcome, team. We're about to dive deep into making our PWAs feel instantaneous. But before we write a single line of optimization code, we need a baseline. We need data. Our compass for this journey is **Lighthouse**.

Lighthouse is an open-source, automated tool built directly into Chrome DevTools. It's designed to audit your web pages and give you a comprehensive report on key areas: Performance, Accessibility, Best Practices, and SEO. For this course, we're zeroing in on that **Performance score**.

Think of Lighthouse as a senior engineer doing a code review on your app's performance. It runs a series of checks against your page during load, measuring key user-centric metrics. It then gives you a score from 0 to 100. But more importantly, it provides a list of actionable "Opportunities" and "Diagnostics" that tell you _exactly_ what to fix to improve your score and, ultimately, your user's experience. This isn't about chasing a perfect score; it's about using a powerful tool to identify the biggest bottlenecks so we can make the most impactful changes.

### **Part 2: Practice - Running Your First Audit**

Let's get our hands dirty.

1. Open any website in a new **Chrome Incognito window**. (This is crucial to prevent your extensions from skewing the results).
2. Open **DevTools** (`Cmd+Option+I` on Mac, `Ctrl+Shift+I` on Windows).
3. Navigate to the **Lighthouse** tab.
4. In the "Categories" section, ensure **Performance** is checked.
5. For "Device", select **Mobile**. This is critical—we must optimize for the most constrained environment first.
6. Click **"Analyze page load"**.

After about a minute, you'll get a report. Let's break down what you're seeing:

- **The Score:** The big number at the top. It's a weighted average of several metrics.
- **The Metrics:** You'll see things like First Contentful Paint, Speed Index, and the all-important Core Web Vitals. We'll dissect these in the next lesson.
- **Opportunities:** This is your to-do list. Lighthouse will suggest specific actions like "Properly size images," "Eliminate render-blocking resources," or "Reduce initial server response time." Each one comes with an estimated time saving.
- **Diagnostics:** This section provides more in-depth information about how your app is performing, highlighting potential issues like large network payloads or excessive DOM size.

Familiarize yourself with this report. It's the map we'll use to guide all our optimization efforts.

## 🧠 **Real-World Case Study: "The E-commerce Homepage Slowdown"**

- **Before:** An online store was seeing a high bounce rate on their homepage, especially on mobile. Conversions were suffering. They didn't know why.
- **The Audit:** They ran a Lighthouse audit and got a performance score of 42. The report's top opportunity was "Serve images in next-gen formats," pointing to a massive 2MB hero banner image in PNG format. This single image was blocking the page from becoming interactive for over 8 seconds on a 3G connection.
- **After:** The team used an online tool to convert the image to a highly compressed WebP format, reducing its size to just 250KB. They re-ran the audit. The performance score jumped to 91. More importantly, their bounce rate dropped by 30%, and mobile conversions increased significantly over the next month. It was a huge win from a simple fix identified by Lighthouse.

## 🤔 **Reflective Questions**

1. Why is it critical to run Lighthouse audits in an Incognito window and with the "Mobile" device setting?
2. Lighthouse provides "Lab Data" (a controlled test). What is the difference between this and "Field Data" (data from real users)? Why are both important?
3. If two pages have the same Lighthouse score of 85, does that mean they provide the same user experience? Why or why not?

## 📚 **Further Reading**

- **Tooling:** [Lighthouse Documentation](https://developer.chrome.com/docs/lighthouse/overview/) (Official Chrome docs).
- **Web.dev:** [An Introduction to Lighthouse](https://www.google.com/search?q=https://web.dev/articles/lighthouse) (A great high-level overview).

## 📝 **Mini Task (Production)**

Your mission is to become an auditor.

1. Pick a complex, well-known website (a major news site, a social media platform, or an e-commerce giant).
2. Run a mobile Lighthouse performance audit on its homepage.
3. Analyze the report and identify the single biggest "Opportunity" that Lighthouse suggests.
4. Write one sentence explaining what the problem is and one sentence describing how you might fix it, based on the Lighthouse recommendation.