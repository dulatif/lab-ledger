# 💡 AT01.2.3: The Resilient Brain: Designing a Persistent State

**Outline:**

- **What to Remember (Presentation):** The critical difference between persistent domain state and transient UI state.
- **The Whitelist and Blacklist (Practice):** Using `redux-persist`'s configuration to selectively store state.
- **Tidying Your App's Memory (Production):** Refactoring a store to persist only what's necessary.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - What to Remember**

Just because we _can_ persist our entire Redux state doesn't mean we _should_. A powerful PWA needs to be resilient, but it also needs to be smart. Persisting everything can lead to strange bugs and a poor user experience.

We need to make a clear distinction between two types of state:

1. **Domain State:** This is the core data of your application. It's the information the user creates or cares about.
    - The items in a shopping cart.
    - The content of a draft email.
    - A user's profile information.
    - **This state should almost always be persisted.**
2. **UI State:** This is temporary state that describes the current condition of the user interface.
    - Is a modal dialog open?
    - Is this data fetch currently in a "loading" state?
    - The current value of a search input field.
    - **This state should almost never be persisted.**

If you persist UI state, you get bizarre behavior. A user refreshes the page and is immediately greeted with a modal dialog from their last session. Or the app gets stuck in a "loading" state forever because that was its condition when the app was last closed. A resilient app should always reset its UI to a clean, predictable state on startup, while gracefully restoring the user's important data.

### **Part 2: Practice - The Whitelist and Blacklist**

`redux-persist` gives us fine-grained control over what gets saved. We can provide a `whitelist` (an array of slice names to persist) or a `blacklist` (an array of slice names to ignore). Using a `whitelist` is generally safer because it forces you to be explicit about what you want to save.

Let's imagine a store with three slices: `cart`, `user`, and `ui`.

```ts
// store.ts
import storage from 'redux-persist/lib/storage';
// ... other imports

const rootReducer = combineReducers({
  cart: cartReducer,   // Domain state - we WANT to persist this
  user: userReducer,   // Domain state - we WANT to persist this
  ui: uiReducer,       // UI state - we DON'T want to persist this
});

// The smart persist config
const persistConfig = {
  key: 'root',
  storage,
  // Only the 'cart' and 'user' slices will be saved to storage.
  // The 'ui' slice will be ignored and will reset on every app load.
  whitelist: ['cart', 'user'],
};

const persistedReducer = persistReducer(persistConfig, rootReducer);

// ... rest of store setup
```

By adding one line (`whitelist: ['cart', 'user']`), we've made our app's memory much tidier and more resilient to bugs caused by stale UI state.

## 🧠 **Real-World Case Study: "The Stuck Spinner"**

- **The Problem:** A PWA for a social media feed had an `api` slice in Redux that managed the status of network requests (e.g., `status: 'pending' | 'fulfilled' | 'rejected'`). The team initially persisted the entire Redux store.
- **The Bug:** A user would be scrolling their feed when their network connection dropped. The app dispatched an action that set the API status to `'pending'`, showing a loading spinner. The user then closed the app. When they re-opened it later (still offline), `redux-persist` would dutifully restore the entire state, including `status: 'pending'`. The app would re-appear with the loading spinner stuck on the screen forever, with no way for the user to escape it.
- **The Solution:** The team identified that the `api` slice was transient UI state. They added `blacklist: ['api']` to their `redux-persist` configuration.
- **The Result:** Now, when the user re-opened the app offline, the persisted domain data (the posts themselves) would load instantly, but the `api` slice would reset to its clean initial state (`status: 'idle'`). The stuck spinner bug was completely eliminated.

## 🤔 **Reflective Questions**

1. Why is using a `whitelist` generally considered a safer practice than using a `blacklist`?
2. Imagine a slice that contains both domain data and UI state (e.g., a `posts` slice with `items: []` and `isLoading: false`). How might you structure your state or use more advanced `redux-persist` features (like nested transforms) to only persist the `items`?
3. What is state rehydration and how can you handle potential version mismatches if you deploy a new version of your app that changes the shape of your persisted Redux state?

## 📚 **Further Reading**

- **Redux Persist Docs:** [State Reconciler](https://www.google.com/search?q=https://github.com/rt2zz/redux-persist%23state-reconciler) (Explains how incoming state is merged).
- **Redux Persist Docs:** [Transforms](https://www.google.com/search?q=https://github.com/rt2zz/redux-persist%23transforms) (For more advanced control, like compressing or encrypting parts of your state).

## 📝 **Mini Task (Production)**

Take the persistent store you built in the last lesson and improve its design.

1. Create a new `uiSlice.ts` that manages a simple boolean flag, e.g., `isSidebarOpen`.
2. Add its reducer to your `rootReducer` in `store.ts`.
3. Modify your `persistConfig` to `blacklist` the new `ui` slice, ensuring it doesn't get saved.
4. In your UI, create a button to toggle the sidebar state and display its status.
5. Verify that you can open the sidebar, increment your persistent counter, and then refresh the page. The counter's value should be remembered, but the sidebar should always be closed on a fresh load.