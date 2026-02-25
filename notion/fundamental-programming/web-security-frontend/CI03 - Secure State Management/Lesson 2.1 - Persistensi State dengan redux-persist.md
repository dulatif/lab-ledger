💡 CI03.2.1: Remembering State: Persistence with redux-persist

**Outline:**

- **The Threat (SEEI):** A frustrating user experience where the application state resets on every page refresh, forcing users to re-login, re-configure their settings, or lose their shopping cart contents.
- **The Defense (PPP):** The principle of "Seamless Sessions." Using a library like `redux-persist` to automatically save the Redux store to a persistent storage (like Local Storage) and rehydrate it when the user returns.
- **Your Mission (Production):** Add `redux-persist` to a basic Redux Toolkit store configuration.

### 📘 Full Lesson Content

### Part 1: Presentation - The Threat (SEEI)

**What is it? (The Vulnerability):**

The "vulnerability" here is not a security flaw, but a **state volatility problem** that leads to a terrible user experience. By default, a Redux store lives only in the browser's memory. When a user closes the tab, hits the refresh button, or their browser crashes, that memory is wiped clean. The entire application state—login status, user preferences, items in a cart—is lost instantly.

**How it Works (The Attack Vector):**

This is the default behavior of any client-side application.

1. A user logs into your application. Their `isAuthenticated` flag is set to `true` in the Redux store.
2. They add several items to their shopping cart. The `cart.items` array in the store is populated.
3. They accidentally press `Cmd+R` (or `Ctrl+R`) to refresh the page.
4. The JavaScript application reloads from scratch.
5. A new, empty Redux store is created with the initial state (`isAuthenticated: false`, `cart.items: []`).
6. The user is now logged out and has an empty cart. This is extremely frustrating and can lead to lost conversions and user churn.

### Part 2: Practice - The Defense (PPP)

**The Golden Rule:**

**"Automate state persistence to create a seamless user experience."** Users expect web applications to remember them. A library like `redux-persist` acts as a bridge between your in-memory Redux store and a persistent storage mechanism in the browser, most commonly `localStorage`. It works in two phases:

1. **Persist:** On every state change, it automatically saves your entire store (or parts of it) to storage.
2. **Rehydrate:** When your application first loads, it reads the saved state from storage and uses it to pre-fill your new Redux store.

**Secure Code Implementation (Basic Setup):**

First, install the library: `npm install redux-persist`

Now, let's integrate it with a Redux Toolkit store.

```ts
// SECURE CODE (FOR UX)
import { configureStore } from '@reduxjs/toolkit';
import { persistStore, persistReducer } from 'redux-persist';
import storage from 'redux-persist/lib/storage'; // defaults to localStorage for web
import rootReducer from './reducers';

// 1. Create the persist config
const persistConfig = {
  key: 'root', // The key for the storage object
  storage,     // The storage engine to use
};

// 2. Create a new "persisted" reducer
const persistedReducer = persistReducer(persistConfig, rootReducer);

// 3. Configure the store to use the persisted reducer
export const store = configureStore({
  reducer: persistedReducer,
  // This is to avoid a non-serializable value error with redux-persist
  middleware: (getDefaultMiddleware) =>
    getDefaultMiddleware({
      serializableCheck: false,
    }),
});

// 4. Create the persistor object
export const persistor = persistStore(store);

```

Finally, in your application's entry point (e.g., `main.jsx` or `index.js`), you wrap your app with the `PersistGate`.

```ts
// In your app's entry point
import { PersistGate } from 'redux-persist/integration/react';
import { store, persistor } from './store';

ReactDOM.render(
  <Provider store={store}>
    <PersistGate loading={null} persistor={persistor}>
      <App />
    </PersistGate>
  </Provider>,
  document.getElementById('root')
);

```

With this setup, your Redux state will now survive page refreshes.

### 🤔 Reflective Questions

1. The `persistConfig` has a `key`. What would happen if two different applications on the same domain (e.g., `example.com/app1` and `example.com/app2`) both used `key: 'root'`?
2. `redux-persist` also has `whitelist` and `blacklist` options in its config. Why would you want to _prevent_ certain parts of your state (like a UI slice with modal states) from being persisted?

### 📚 Further Reading

- **Redux Persist:** [GitHub Repository & Documentation](https://github.com/rt2zz/redux-persist) - The official documentation is the best resource.

### 📝 Mini Task (Production)

You have a Redux store with three slices: `auth`, `cart`, and `ui`. You only want to persist the `auth` and `cart` slices, but not the `ui` slice.

Your task: Write the `persistConfig` object that would achieve this using the `whitelist` option.