# 💡 AD01.2: Taking the Wheel: Switching to `injectManifest`

**Outline:**

- **Why We Need More Control (Presentation):** Understanding the limitations that force the switch.
- **The Refactoring Process (Practice):** A step-by-step guide to modifying your build configuration.
- **Your First Custom SW (Production):** Applying the switch to your own project.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - Why We Need More Control**

We've established that `generateSW` is a great starting point. But you'll quickly hit a ceiling. The moment you want to do more than just precache your static files, you need to take over.

When do you need more control?

- **Caching API Data:** You want to cache data from your backend. For example, caching `/api/projects` so the user can still see their project list offline.
- **Custom Offline Fallbacks:** If a user is offline and tries to navigate to a page they've never visited, you don't want the browser's ugly "No Internet" screen. You want to show a gracefully designed, custom offline page from your cache.
- **Handling Push Notifications:** This requires custom event listeners inside the service worker. `generateSW` won't let you add them.
- **Complex Caching Strategies:** Maybe you want to cache images from a CDN differently than you cache fonts from Google Fonts. This requires multiple, specific routing rules.

Switching to `injectManifest` isn't just a preference; it's a necessity for building a PWA that feels seamless and truly integrated with the device. It's the move from a "website that works offline" to a "web application that is network-independent."

### **Part 2: Practice - The Refactoring Process**

Let's walk through the exact changes needed in your `vite.config.ts` to make the switch. It's a small change in code, but a big change in philosophy.

**Step 1: The "Before" State (`generateSW`)** Here's our starting point. Simple and declarative.

```ts
// vite.config.ts (BEFORE)
import { VitePWA } from 'vite-plugin-pwa';

export default {
  plugins: [
    VitePWA({
      registerType: 'autoUpdate',
      workbox: {
        globPatterns: ['**/*.{js,css,html,ico,png,svg}'],
        runtimeCaching: [ // Some runtime caching is possible, but limited
          {
            urlPattern: /^https:\\/\\/fonts\\.googleapis\\.com\\/.*/i,
            handler: 'CacheFirst',
            options: { cacheName: 'google-fonts-cache' }
          }
        ]
      }
    })
  ]
}
```

**Step 2: The "After" State (`injectManifest`)** Now, we take control. We tell the plugin what our strategy is and where to find our source file.

```ts
// vite.config.ts (AFTER)
import { VitePWA } from 'vite-plugin-pwa';

export default {
  plugins: [
    VitePWA({
      registerType: 'autoUpdate',
      strategies: 'injectManifest', // 1. Change strategy to 'injectManifest'
      srcDir: 'src',                // 2. Specify the source directory
      filename: 'sw.ts'             // 3. Name our new service worker source file
      // 4. The 'workbox' object is no longer needed here for routing!
      //    All that logic will now live inside 'src/sw.ts'.
    })
  ]
}
```

That's it for the configuration. The next step, which we'll cover in the next lesson, is to actually create the `src/sw.ts` file and start writing our own logic.

## 🧠 **Real-World Case Study: "The Unreliable Conference Wi-Fi"**

- **Before (`generateSW`):** A company built an event schedule PWA for a conference. It used `generateSW` to cache the app shell. On day one, the conference Wi-Fi is overloaded and slow. When attendees open the app, the main shell loads, but the API call to `/api/schedule` fails, showing a loading spinner forever. The app is technically "offline-capable" but not "offline-useful."
- **After (`injectManifest`):** For the next conference, the developers switch to `injectManifest`. They write a custom route in their `sw.ts` that caches the `/api/schedule` endpoint with a `StaleWhileRevalidate` strategy. Now, when the Wi-Fi is spotty, the service worker instantly serves the _stale_ schedule from the cache (so the user sees _something_ immediately), while simultaneously trying to fetch a fresh version in the background. When the new version arrives, the UI silently updates. The user experience is transformed from frustrating to seamless.

## 🤔 **Reflective Questions**

1. In the `injectManifest` config, why is the `workbox.runtimeCaching` property removed? Where does that logic go now?
2. What does `srcDir: 'src'` and `filename: 'sw.ts'` tell the build tool to do?
3. What's the first piece of custom logic you would want to implement after switching to `injectManifest` for a social media feed app?

## 📚 **Further Reading**

- **Tooling:** [Vite PWA Plugin - `injectManifest` options](https://vite-pwa-org.netlify.app/guide/inject-manifest.html)
- **Workbox Docs:** [Getting Started with `injectManifest`](https://www.google.com/search?q=https://developer.chrome.com/docs/workbox/modules/workbox-build/%23getting-started-with-injectmanifest)
- **Web.dev:** [The App Shell Model](https://www.google.com/search?q=https://web.dev/articles/app-shell) (A core concept enabled by custom service workers).

## 📝 **Mini Task (Production)**

Take the project you've been working on and perform the refactor.

1. Open your `vite.config.ts` (or equivalent build configuration file).
2. Change the PWA plugin configuration from `generateSW` to use `injectManifest`.
3. Set the `srcDir` and `filename` properties appropriately.
4. Run your build command. It might fail or your PWA might not work correctly—that's expected! Why? Because you haven't created the source service worker file yet. The key takeaway is to get comfortable with the configuration change itself. We'll fix the build in the next lesson.