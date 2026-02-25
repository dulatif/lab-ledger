# 💡 AM01.4: The Guardrails: Setting a Performance Budget

**Outline:**

- **Staying Fast (Presentation):** Moving from one-time fixes to a culture of sustained performance.
- **Calculating Your Budget (Practice):** A hands-on guide to defining limits for your application.
- **Enforcing the Budget (Production):** Integrating performance checks into your development workflow.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - Staying Fast**

So far, we've learned how to diagnose problems. But optimization isn't a one-time task. It's a continuous discipline. The biggest threat to a fast application is "feature creep"—the slow, gradual addition of new libraries, larger images, and more complex components that, over time, erode performance.

How do we prevent this? We set **Performance Budgets**.

A performance budget is a set of hard limits that your team agrees not to exceed. It's a guardrail that protects your users from slow experiences. Instead of asking "Is the app fast enough?" after a feature is built, the budget forces you to ask "Can we build this feature _within our budget_?" before you even start.

This shifts performance from an afterthought to a core part of the design and development process. It creates a shared responsibility for performance across the entire team and prevents the slow decline that plagues so many projects.

There are two main types of budgets:

- **Quantity-based:** Simple limits on asset sizes. (e.g., "Our total JavaScript size must not exceed 170KB gzipped").
- **Milestone-based:** Limits based on user-centric metrics. (e.g., "Our LCP must be under 2.5 seconds on a slow 4G connection").

We'll start with quantity-based budgets because they are easier to measure and enforce.

### **Part 2: Practice - Calculating Your Budget**

Let's define a reasonable budget. Our goal is to be interactive on a median mobile device over a simulated "Fast 3G" connection in **under 5 seconds**. This is a great starting point for most applications.

To do this, we can use a tool like [Performance Budget Calculator](https://www.performancebudget.io/). But the principle is simple. We have a time budget (5000ms) and we allocate it to different resources. A widely accepted, battle-tested budget for this goal is the following:

- **Total JavaScript:** max **170 KB** (gzipped)
- **Total CSS:** max **100 KB** (gzipped)
- **Images:** max **400 KB**
- **HTML:** max **50 KB**
- **Fonts:** max **100 KB**

Why 170KB for JavaScript? On a typical mid-range phone, it takes the browser roughly 1 second to download (on Fast 3G), parse, and compile 170KB of gzipped JavaScript. This leaves us with 4 seconds for everything else.

Your first step is to measure your own application against this budget. Use your browser's Network tab (with the "Disable cache" box checked) to see the total size of your assets on a clean load. How do you stack up? This exercise immediately highlights the biggest areas for improvement.

## **🧠 Real-World Case Study: "The Feature Creep Catastrophe"**

- **The Problem:** A successful SaaS PWA started out fast. Over two years, they added dozens of features, a new analytics provider, a customer support chat widget, A/B testing tools, and more. Each addition seemed small, but they never measured the cumulative impact. Their initial JS bundle swelled from 150KB to over 1.2MB. The app now took 15+ seconds to become interactive on mobile.
- **The Budget Mandate:** The company declared a "performance freeze." No new features could be added until they got back under budget. They spent an entire quarter on a "performance diet," implementing code splitting, removing unused libraries, and replacing the chat widget with a lighter alternative.
- **The Guardrail:** After the painful diet, they integrated a budget check into their CI/CD pipeline. Now, if a developer's pull request pushes the total bundle size over the 170KB limit, the build fails automatically. The PR cannot be merged until the performance issue is addressed. This automated guardrail ensures they will never suffer from performance creep again.

## 🤔 **Reflective Questions**

1. Which do you think is more effective for a development team: a quantity-based budget (e.g., KB size) or a milestone-based budget (e.g., LCP time)? Why?
2. Your designer wants to add a new custom font and a video background to the homepage. How does having a performance budget change the conversation you have with them?
3. How can you automate the process of checking your performance budget? (Hint: think about CI/CD and tools like `bundlesize` or Lighthouse CI).

## 📚 **Further Reading**

- **Web.dev:** [Performance Budgets 101](https://web.dev/articles/performance-budgets-101) (A fantastic introduction).
- **Tooling:** [bundlesize](https://github.com/siddharthkp/bundlesize) (A simple tool to check bundle size in your CI).
- **Tooling:** [Lighthouse CI](https://www.google.com/search?q=https://developer.chrome.com/docs/lighthouse/ci/) (Automate running Lighthouse on every commit).

## 📝 **Mini Task (Production)**

Time to put your own app on the scale.

1. Define a simple performance budget for your application using the example values from the "Practice" section.
2. Open your app's production build in a new Incognito window.
3. Open the **Network** tab in DevTools, check "Disable cache," and reload the page.
4. At the bottom of the Network panel, it will show the total size of transferred resources. Use the filters (JS, CSS, Img, etc.) to see the total size for each category.
5. Compare your app's current state to the budget. Which category is the most "over budget"? This is your primary target for optimization in the coming modules.