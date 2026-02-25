💡 CI01-2.1: Your Performance Compass

Outline:

- **The Metric & The Bottleneck (Presentation):** Understanding what Lighthouse is and its five audit categories.
- **The Optimization Technique (Practice):** A high-level tour of the Lighthouse report structure.
- **Your Performance Mission (Production):** Identifying the five key scores for any website.

📘 **Full Lesson Content**

### **Part 1: Presentation - The Metric & The Bottleneck**

- **The Goal:** Today, we're getting to know our primary diagnostic tool: **Lighthouse**. Think of it as an automated code reviewer for web quality. It runs a series of audits against a page and generates a report on how well it did.
- **Identifying the Problem:** Before you can fix performance issues, you need a consistent way to find them. The bottleneck is a lack of data. Without a tool like Lighthouse, performance is just a matter of opinion ("it feels slow"). Lighthouse replaces subjective feelings with objective scores and actionable recommendations, giving us a starting point for our optimization work.

### **Part 2: Practice - The Optimization Technique**

- **The Principle:** The technique is to understand the scope of Lighthouse. It's not just about speed. It audits five distinct categories, giving you a holistic view of your page's quality.
    
- **A Tour of the Five Categories:**
    
    1. **Performance (Our Main Focus):** Scores your page on metrics like LCP, TBT, and CLS. This is where we'll spend most of our time. A score of 90-100 is the goal.
    2. **Accessibility (A11y):** Checks for issues that prevent users with disabilities from using your site, like missing alt text for images or poor color contrast.
    3. **Best Practices:** Looks for common web development mistakes, such as using insecure libraries or not using HTTPS.
    4. **SEO:** Runs basic checks to ensure your page is discoverable by search engines, like having a valid `title` tag and a `meta` description.
    5. **Progressive Web App (PWA):** Audits your page against the PWA checklist, checking for things like a service worker and a web app manifest.
    
    Understanding these categories helps you see performance as part of a bigger picture of creating high-quality web applications.
    

### 🧠 **Real-World Case Study: "The Quick Check-up"**

- **The Scenario:** A startup is about to launch a new landing page. The engineers think it's ready, but the product manager is worried it might have issues they've overlooked.
- **The Implementation:** Before deploying, an engineer runs a quick Lighthouse audit. The performance score is a decent 85, but the Accessibility score is a low 60, and the SEO score is 75.
- **The Takeaway:** The report immediately flags critical, non-performance issues that were missed. For example, the main call-to-action button had poor color contrast (bad A11y), and the page was missing a meta description (bad SEO). Lighthouse acted as an automated quality assurance (QA) step, catching simple but costly mistakes before they reached users and search crawlers.

### 🤔 **Reflective Questions**

1. Why is it important for a performance-focused engineer to also pay attention to the Accessibility and SEO scores?
2. Lighthouse gives you a score from 0 to 100. How might this scoring system be useful when communicating with non-technical stakeholders like product managers?
3. Based on the five categories, what kind of tool would you say Lighthouse is? Is it just for performance engineers?

### 📚 **Further Reading**

- **Web Vitals:** [An introduction to Core Web Vitals on web.dev](https://web.dev/articles/vitals)
- **Lighthouse Docs:** [Official Lighthouse Documentation](https://developer.chrome.com/docs/lighthouse/)
- **Scoring Guide:** [How the Lighthouse performance score is calculated](https://developer.chrome.com/docs/lighthouse/performance/performance-scoring)

### 📝 **Mini Task (Production)**

- **Your Mission:** You don't need to run an audit yet. Simply find a public Lighthouse report online (you can search for "Lighthouse report example" or use a site like [web.dev/measure](https://www.google.com/search?q=https://web.dev/measure) on a known site).
- **Your Deliverable:** Look at the report and list the five scores for the five main categories (Performance, Accessibility, Best Practices, SEO, PWA). This task is just to familiarize yourself with what the final output looks like.