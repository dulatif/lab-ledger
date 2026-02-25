# 💡 AD01.1: Workbox Configuration: Autopilot vs. Manual Control

**Outline:**

- **The Two Philosophies (Presentation):** Understanding `generateSW` and `injectManifest`.
- **Configuration in Practice (Practice):** Seeing how each mode looks in a build tool configuration.
- **Making the Call (Production):** Deciding which strategy fits your project's needs.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Two Philosophies**

Alright team, let's talk about how Workbox operates. Think of it as having two modes for flying a plane: autopilot and manual control.

- **`generateSW` (Autopilot):** This is the "magic" mode. You tell your build tool (like Vite or Webpack) a few rules, and it generates a complete service worker file for you. It's fantastic for getting started quickly and for simple sites where all you need is precaching of your static assets (JS, CSS, images). It's simple, declarative, and hard to mess up. The major limitation? You can't add your own custom logic. What it generates is a black box.
- **`injectManifest` (Manual Control):** This is where you, the engineer, take the wheel. You write your own service worker file from scratch. The build tool's job is simply to take that file, find a special placeholder, and "inject" the list of files to precache. This gives you ultimate power and flexibility. You can add custom routing logic, push notification handlers, background sync—anything the Service Worker API allows. This is the mode for professional-grade PWAs. We start with autopilot to learn the basics, but we graduate to manual control to build truly resilient, app-like experiences.

### **Part 2: Practice - Configuration in Practice**

Let's see what these look like in a `vite.config.ts` file using the `vite-plugin-pwa`.

**Example 1: `generateSW` Configuration** This is a simple, set-it-and-forget-it approach.

```ts
// vite.config.ts
import { VitePWA } from 'vite-plugin-pwa';

export default {
  plugins: [
    VitePWA({
      workbox: {
        // Simple configuration. Workbox generates sw.js for us.
        globPatterns: ['**/*.{js,css,html,ico,png,svg}']
      }
    })
  ]
}
```

**Example 2: `injectManifest` Configuration** Notice how we now point to our _source_ service worker file.

```ts
// vite.config.ts
import { VitePWA } from 'vite-plugin-pwa';

export default {
  plugins: [
    VitePWA({
      strategies: 'injectManifest', // We explicitly choose the mode
      srcDir: 'src',                // Where our source code lives
      filename: 'sw.ts'             // The name of our source service worker file
    })
  ]
}
```

The build process will now read `src/sw.ts`, compile it, inject the precache manifest, and output the final `sw.js` in your distribution folder.

## 🧠 **Real-World Case Study: "The Evolving App"**

- **Startup Phase (`generateSW`):** A new SaaS product launches its marketing website. It's mostly static content. Using `generateSW` is perfect. It takes 10 minutes to set up, and now the site loads offline for potential customers who revisit it. Low effort, high impact.
- **Growth Phase (`injectManifest`):** The product is a hit and evolves into a complex single-page application. The team now needs to cache API responses with a `StaleWhileRevalidate` strategy, handle push notifications for new messages, and provide a custom fallback page when API calls fail offline. `generateSW` can't do this. They _must_ switch to `injectManifest` to write this custom logic and deliver the reliable, app-like experience their users expect.

## 🤔 **Reflective Questions**

1. Why is `generateSW` considered a "black box"? What are the practical implications of not being able to edit the output file directly?
2. If you use `injectManifest`, what is the build tool's _only_ responsibility regarding the service worker?
3. Could you start a project with `generateSW` and switch to `injectManifest` later? What would be the trigger for making that switch?

## 📚 **Further Reading**

- **Tooling:** [Vite PWA Plugin - `generateSW` vs `injectManifest`](https://www.google.com/search?q=https://vite-pwa-org.netlify.app/guide/generate-sw-vs-inject-manifest.html)
- **Workbox Docs:** [Comparing `generateSW` and `injectManifest`](https://www.google.com/search?q=https://developer.chrome.com/docs/workbox/modules/workbox-build/%23which-tool-to-use)
- **Web.dev:** [An Intro to Service Workers](https://www.google.com/search?q=https://web.dev/articles/service-workers-introduction)

## 📝 **Mini Task (Production)**

Your task is to play the role of a code reviewer.

1. Look at the two `vite.config.ts` examples in the "Practice" section above.
2. In a few sentences, explain to a junior developer which configuration you would recommend for a simple portfolio website versus a complex, data-driven application like a project management tool. Justify your choice based on the principles of control and simplicity.