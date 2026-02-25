# 💡 AD01.4.2: The Signal: Detecting New Versions in Your App

**Outline:**

- **Bridging the Gap (Presentation):** How the client-side app can communicate with the service worker registration.
- **Listening for Updates (Practice):** Using the `vite-plugin-pwa` virtual module to detect a waiting worker.
- **Logging the Signal (Production):** Implementing the detection logic in your main React component.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - Bridging the Gap**

So, we know a new service worker can be in the `waiting` state. That's great for the browser, but it's invisible to the user. Our client-side application—our React code—is completely unaware that a fresh version of the app is ready to go.

How do we bridge this gap? We need a signal. We need the service worker registration process to notify our application code when an update is available.

Luckily, modern PWA build tools provide a clean way to do this. For example, `vite-plugin-pwa` offers a "virtual module" that you can import into your React code. This module handles the complexity of checking the service worker registration's state and exposes simple, reactive flags or callbacks that you can use. The most important one is the signal that tells us: "A new version has been downloaded and is waiting."

Once we can detect this signal within our React components, we can then build UI to inform the user and give them control over the update process.

### **Part 2: Practice - Listening for Updates**

The `vite-plugin-pwa` makes this remarkably simple with its `virtual:pwa-register` module. It exposes a function, `registerSW`, that we can use in our main application file (`main.tsx` or `index.tsx`). This function can be configured to call back when an update is detected.

Let's see the code.

```
// src/main.tsx or your main entry file

import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App';
import { registerSW } from 'virtual:pwa-register';

// This function returns its own update function, which we can call later
const updateSW = registerSW({
  onNeedRefresh() {
    // This callback is triggered when a new service worker is waiting.
    console.log('A new version is available and waiting to be installed!');
    // In the next lesson, we'll show a UI prompt here instead of just logging.
  },
  onOfflineReady() {
    // This callback is triggered when the app is ready to work offline.
    console.log('The app is ready to work offline.');
  },
});

ReactDOM.createRoot(document.getElementById('root')!).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
```

**Breaking it down:**

- `import { registerSW } from 'virtual:pwa-register'`: This special import is made available by the Vite plugin.
- `registerSW({ ... })`: We call this function and pass it an object with callback handlers.
- `onNeedRefresh()`: This is the golden ticket. The plugin will automatically invoke this function for us when it detects a waiting service worker. It's the signal we've been looking for.
- `onOfflineReady()`: This is a helpful bonus, letting us know when the initial precaching is complete.

Now, if you repeat the process from the last lesson (run the app, change the service worker, rebuild, refresh), you won't just see the waiting worker in DevTools—you'll also see our "A new version is available..." message logged in the console!

## 🧠 **Real-World Case Study: "The Silent Failure"**

- **Before (No Detection):** A financial services company deploys a critical security patch for their PWA. They tell their users to "please refresh the app to get the latest update." Many users don't, or they only close and reopen their browser days later. For days, a significant portion of their user base is running an old, less secure version of the code without anyone knowing.
- **After (With Detection):** The developers implement the `onNeedRefresh` callback. When the security patch is deployed, the _next time_ a user opens the app, the callback is triggered. This allows the app to show a prominent, un-dismissible dialog: "A critical security update is required. Please save your work and click 'Update Now' to continue." This proactive approach ensures that critical updates are applied promptly, dramatically improving the security posture of the application.

## 🤔 **Reflective Questions**

1. Why is the import path `virtual:pwa-register` called "virtual"? What does that imply about where that file exists in your project?
2. What is the difference in purpose between the `onNeedRefresh` and `onOfflineReady` callbacks?
3. How does this client-side detection mechanism change the user experience of updating a PWA compared to a traditional website?

## 📚 **Further Reading**

- **Tooling:** [Vite PWA Plugin - `registerSW` options](https://vite-pwa-org.netlify.app/guide/prompt-for-update.html) (The official documentation for this feature).
- **A Deeper Dive:** [This article explains the underlying `ServiceWorkerRegistration` API events (`updatefound`) that these plugins abstract away](https://whatwebcando.today/articles/handling-service-worker-updates/).
- **UX Patterns:** [Patterns for PWA Updates](https://www.google.com/search?q=https://medium.com/dev-channel/service-worker-updates-3424d546255d) (Discusses different UI/UX approaches for update notifications).

## 📝 **Mini Task (Production)**

Implement the update detection logic in your application.

1. Make sure your project is configured to use `vite-plugin-pwa` (or an equivalent for your build tool).
2. Go to your main application entry point (e.g., `main.tsx`).
3. Import `registerSW` from `virtual:pwa-register`.
4. Call the `registerSW` function, providing an `onNeedRefresh` callback that logs a message to the console.
5. Test the entire flow: run the app, make a change to `src/sw.ts`, rebuild, and refresh the app. Verify that your message appears in the console.