# 💡 CD03-3.2: Precaching Static Assets on Install [not completed]

**Outline:**

- **The Modern Web App (Presentation):** The "App Shell" model and why we cache it upfront.
- **The Engine Room (Practice):** Using the `install` event to populate the cache.
- **Your First Cached App (Production):** Time to store your app's core files for offline use.

## 📘 **Full Lesson Content**

### Part 1: Presentation - The App Shell Model

The first step to a reliable offline experience is to ensure the core user interface—the "shell" of your application—is always available. The **App Shell** consists of the minimal HTML, CSS, and JavaScript required to power the UI. It's the part of your app that doesn't change often. Think of the header, the navigation bar, the main layout structure, and the core JS bundle.

The strategy is simple: we will download and save these core assets to the cache the very first time a user visits. This is called **precaching**.

The perfect time to do this is during the service worker's `install` event. Remember, this event fires only once per service worker version, making it the ideal moment for setup tasks. By precaching the app shell, we guarantee that the application can at least _load_ and show its interface, even if the user has no network connection on subsequent visits.

### Part 2: Practice - Caching in the `install` Event

We will use the `event.waitUntil()` method inside our `install` listener. This method takes a promise and tells the service worker's lifecycle to not proceed with installation until that promise resolves. If the promise rejects (e.g., one of the files fails to download), the installation will fail, and the service worker won't be activated. This is a crucial guarantee—it means our app is only considered "installed" if the entire shell is successfully cached.