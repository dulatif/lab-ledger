# 💡 CD01-1.3: PWA vs. Native: Choosing the Right Tool

**Outline:**

- **The Modern App Landscape (Presentation):** Understanding the strengths and weaknesses of each platform.
- **The Decision Framework (Practice):** How to decide which approach is right for your project.
- **Make Your Pitch (Production):** Argue for a PWA in a real-world business scenario.

## 📘 **Full Lesson Content**

### Part 1: Presentation - The Modern App Landscape

One of the most common questions is: "Should we build a PWA or a native app?" The answer isn't that one is better than the other. The correct answer is: **"It depends."** They are different tools for different jobs. Let's break down the core trade-offs.

|Feature|Progressive Web App (PWA)|Native App (iOS/Android)|
|---|---|---|
|**Reach & Discovery**|**Massive.** Accessible via a URL. Discoverable via search engines.|**Limited.** Must be found and installed via an app store.|
|**Installation**|**Frictionless.** "Add to Home Screen" is a single tap. No store.|**High Friction.** Search store, tap install, wait, open.|
|**Development**|**Single Codebase.** One app runs on all platforms (web, Android, iOS, desktop).|**Multiple Codebases.** Often requires separate teams/code for iOS and Android.|
|**Updates**|**Instant.** Update the code on your server, users get it on next load.|**Slow.** Submit to app store, wait for review, user must manually update.|
|**Hardware Access**|**Growing.** Good access to camera, GPS, notifications. Limited access to contacts, Bluetooth, etc.|**Complete.** Full, unrestricted access to all device hardware and OS features.|
|**Performance**|**Excellent** for most apps. Slower for graphically intense or CPU-heavy tasks.|**Highest Possible.** Direct access to the OS gives maximum performance for games, video editing, etc.|
|**App Store "Tax"**|**None.** You control distribution and are not subject to a 30% cut of revenue.|**Yes.** Major app stores take a significant cut of all digital sales.|

The core takeaway: **PWAs excel in reach, speed of development, and ease of access. Native apps excel in raw performance and deep hardware integration.**

### Part 2: Practice - The Decision Framework

So, how do you choose? Ask yourself these questions about your project:

1. **Is deep hardware integration critical?**
    - _Example:_ You're building a photo editing app that needs advanced background processing and direct access to the file system. **=> Lean Native.**
    - _Example:_ You're building an e-commerce site. **=> Lean PWA.**
2. **Is instant access and discoverability a priority?**
    - _Example:_ You're a news organization that wants users to find your articles on Google and instantly start reading. **=> Lean PWA.**
    - _Example:_ You're building a secure banking app where users expect to find you in the app store. **=> Lean Native.**
3. **How important is a single codebase and rapid iteration?**
    - _Example:_ You're a startup with a small team and need to launch and update your product quickly across all platforms. **=> Lean PWA.**
    - _Example:_ You're a large company with separate, dedicated iOS and Android teams. **=> Either could work.**

This isn't about PWA _or_ Native. Many companies use a hybrid approach. They build a PWA for broad reach and core functionality, and a native app for their power users who need more advanced features.

## 🧠 **Real-World Case Study: "Pinterest's PWA Strategy"**

Pinterest's old mobile web experience was slow and clunky. They found that only 1% of mobile web users converted into sign-ups or app installs. They needed to provide a better experience without forcing a high-friction native app download.

They built a PWA.

- **The Result:** Core engagements shot up by 60%. User-generated ad revenue increased by 44%. Time spent on the site increased by 40%.
- **The "Why":** The PWA delivered a fast, app-like experience _instantly_ via the browser. Users could try it out with zero commitment. Once they saw the value, the prompt to "Add to Home Screen" was a much smaller ask than "Go to the app store and download our 50MB app." It perfectly bridged the gap between web and native.

## 🤔 **Reflection Questions**

1. What kind of application is a "bad" fit for a PWA? What characteristics would it have?
2. How does the "app store tax" (the 15-30% revenue cut) influence the business decision to build a PWA vs. Native?
3. As the web platform gets more "Capable," do you think the need for native apps will decrease over time? Why or why not?

## 📚 **Further Reading**

- **Web.dev:** [PWA vs Native: Which is right for you?](https://www.google.com/search?q=https://web.dev/articles/pwa-vs-native) (Note: This is a conceptual link, the actual article may vary, but the topic is key).

## 📝 **Mini-Task (Production)**

You are a principal engineer at a fast-growing online clothing retailer. Your task is to write a short pitch (2-3 sentences) to your CEO, who is not technical, arguing why a PWA is a better initial investment than building separate iOS and Android native apps.

Focus on the business benefits: reach, cost, and user acquisition.