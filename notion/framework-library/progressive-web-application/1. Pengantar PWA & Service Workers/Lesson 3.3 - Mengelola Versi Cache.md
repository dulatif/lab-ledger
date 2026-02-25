# 💡 CD03-3.3: Managing Cache Versions [not completed]

**Outline:**

- **The Modern Web App (Presentation):** Why you must clean up old caches.
- **The Engine Room (Practice):** Using the `activate` event to manage cache storage.
- **Your First Cache Migration (Production):** Time to update your app and gracefully handle the old version.

## 📘 **Full Lesson Content**

### Part 1: Presentation - The Cleanup Crew

You've just deployed a new version of your application with updated styles and logic. Your build process has generated new `app.b4c3d2a1.js` and `styles.x9y8z7w6.css` files. Your new service worker (`v2`) dutifully installs and, in its `install` event, caches these new files in a new cache, `app-shell-v2`.

But what about the old `app-shell-v1` cache? It's still sitting there, taking up space on the user's device with the outdated assets. If you don't clean it up, you will accumulate gigabytes of old files over time.

This is not only wasteful; it can also cause bugs if an old service worker is somehow reactivated and serves stale assets.

The **`activate`** event is the perfect time to handle this cleanup. This event fires when a new service worker has installed and is ready to take control from the previous version. This is your signal to safely remove any caches that don't belong to the current version.

### Part 2: Practice - Cleaning Up in the `activate` Event

The strategy is straightforward:

1. Define a list of "allowed" cache names (usually just the current cache).
2. In the `activate` event, get a list of all existing cache names.
3. Loop through them and delete any cache that is not on your "allowed" list.

Just like with `install`, we wrap this logic in `event.waitUntil()` to ensure the browser doesn't terminate our service worker mid-cleanup.