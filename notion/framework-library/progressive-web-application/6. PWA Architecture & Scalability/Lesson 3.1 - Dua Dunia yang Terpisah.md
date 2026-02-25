# 💡 AT01.3.1: The Great Divide: Two Separate Worlds

**Outline:**

- **The Wall (Presentation):** Understanding the fundamental separation between your app's UI thread and the Service Worker thread.
- **Different Toolkits (Practice):** A conceptual look at what APIs are available in each "world."
- **The Forbidden Access (Production):** A practical experiment to prove the separation exists.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Wall**

Team, we need to internalize a fundamental concept that governs all advanced PWA features: your main application and your service worker live in **two completely separate worlds**. They operate on different threads, have different global scopes, and have a strict wall between them.

1. **The UI World (`window`):** This is your React application. It runs on the main browser thread. It has access to the `window` object, the DOM (`document`), and all the variables and state you've defined in your components. Its primary job is to render pixels on the screen and respond to user input.
2. **The Service Worker World (`self`):** This is your `sw.js` file. It runs on a separate, background thread. It has no access to the DOM. If you try to use `window` or `document` here, it will crash. Its global scope is `self`, which refers to the `ServiceWorkerGlobalScope`. Its job is to be a network proxy—intercepting fetches, managing caches, and handling push notifications.

This separation is a critical security and performance feature. By running on a background thread, the service worker can do its work without ever blocking or freezing your UI. By not having DOM access, it prevents a whole class of potential security vulnerabilities. But this separation also means we need a special way for them to talk to each other.

### **Part 2: Practice - Different Toolkits**

Think of it like two different workshops.

**The UI Workshop (`window` context):**

- **Main Tool:** The DOM. You can create, read, update, and delete elements.
- **Power Source:** User interaction and component lifecycle (`useEffect`, `useState`).
- **Knowledge:** Knows everything about what's currently on the screen.
- **Blind Spot:** Has no direct control over the network cache or what happens when the app is closed.

**The Service Worker Workshop (`self` context):**

- **Main Tool:** Event Listeners (`fetch`, `push`, `sync`, `install`, `activate`).
- **Power Source:** Browser events, even when the app's tab is closed.
- **Knowledge:** Knows everything about network requests and caches.
- **Blind Spot:** Has no idea what the UI looks like or what components are rendered.

[Gambar a server room with network cables](https://encrypted-tbn1.gstatic.com/licensed-image?q=tbn:ANd9GcTRca_NVH_RY7rfsvlw4igflSijzp7xnP2fjMRQsfAm0pYRYlhMVLzW7mEzGx1qhZ_xRA0Xg9GZDMbLhlkUs1MFW8ySmu2ByfUMrse7M7ozlzzx-_w)

Getty Images

They are both powerful, but they are specialists with completely different tools and purposes. To build a great PWA, we need to make these two specialists collaborate effectively.

## 🧠 **Real-World Case Study: "The Crashing Service Worker"**

- **The Problem:** A developer was trying to implement a feature where a push notification would directly update a counter displayed in the app's header. They wrote code inside the service worker's `'push'` event listener that looked like this: `const counter = document.getElementById('notification-counter'); counter.textContent = '...';`
- **The Bug:** The service worker would fail to install, and push notifications wouldn't work at all. Looking at the console in the DevTools Application tab, they saw the error: `Uncaught ReferenceError: document is not defined`.
- **The Realization:** They had a "eureka" moment. The service worker is a background script; it has no `document`. It's a completely different context from the UI. They realized they couldn't directly manipulate the DOM from the service worker. They needed a communication channel to _tell_ the UI to update itself.

## 🤔 **Reflective Questions**

1. Why is it a good thing for performance that the service worker runs on a separate thread?
2. If you can't access `window` in a service worker, how do you handle things you'd normally use it for, like timers (`setTimeout`)? (Hint: `self` has many of the same methods).
3. What are the security implications if a service worker _did_ have access to the DOM?

## 📚 **Further Reading**

- **MDN:** [Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API) (The root of all service worker knowledge).
- **MDN:** [ServiceWorkerGlobalScope](https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorkerGlobalScope) (A list of all properties and methods available on `self` in a service worker).

## 📝 **Mini Task (Production)**

Let's prove the wall exists.

1. Open your service worker file (`src/sw.ts` or similar).
2. Add the following line anywhere in the global scope of the file: `console.log(document);`
3. Save the file and go to your running PWA. Open the Chrome DevTools.
4. Go to the "Application" tab, then the "Service Workers" section. You should see an error next to your newly installed service worker script.
5. Click on the "error" link. It will take you to the console, where you will see the `ReferenceError: document is not defined`. This is the proof that the two worlds are separate. Remove the line of code once you've seen the error.