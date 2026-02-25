# 💡 AT01.2.2: The Predictable Brain: Setting Up Redux Toolkit & Persist

**Outline:**

- **Modern Redux (Presentation):** Introducing Redux Toolkit as the standard for efficient state management and `redux-persist` for storage.
- **Wiring the Brain (Practice):** A step-by-step guide to configuring a persistent Redux store.
- **Your First Persistent Slice (Production):** Building a simple counter that remembers its value after a refresh.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - Modern Redux**

For years, Redux was the king of state management, but it was known for having a lot of "boilerplate" code. That's no longer the case. **Redux Toolkit (RTK)** is the official, opinionated, "batteries-included" toolset for modern Redux development. It simplifies store setup, eliminates boilerplate, and includes the most common Redux addons by default. We will always use RTK.

But RTK on its own is still just an in-memory store. To give our PWA a long-term memory, we pair it with a powerful library called **`redux-persist`**. This library acts as a bridge between your Redux store and a storage engine (like `localStorage` or the more robust `IndexedDB`). With just a few lines of configuration, it automatically saves your entire Redux state to storage whenever it changes and rehydrates it when the app loads.

The combination of RTK (for clean, predictable state logic) and `redux-persist` (for effortless persistence) is the architectural backbone for a scalable, offline-first PWA.

### **Part 2: Practice - Wiring the Brain**

Let's set up a persistent store from scratch.

**Step 1: Installation**

```bash
npm install @reduxjs/toolkit react-redux redux-persist
```

**Step 2: Create a simple slice** A "slice" is a collection of reducer logic and actions for a single feature.

```ts
// features/counter/counterSlice.ts
import { createSlice } from '@reduxjs/toolkit';

const counterSlice = createSlice({
  name: 'counter',
  initialState: { value: 0 },
  reducers: {
    increment: (state) => {
      state.value += 1;
    },
  },
});

export const { increment } = counterSlice.actions;
export default counterSlice.reducer;
```

**Step 3: Configure the persistent store** This is where we bring RTK and `redux-persist` together.

```ts
// store.ts
import { configureStore, combineReducers } from '@reduxjs/toolkit';
import { persistStore, persistReducer } from 'redux-persist';
import storage from 'redux-persist/lib/storage'; // defaults to localStorage
import counterReducer from './features/counter/counterSlice';

// 1. Combine your reducers
const rootReducer = combineReducers({
  counter: counterReducer,
});

// 2. Create the persist config
const persistConfig = {
  key: 'root', // The key for the root of the storage
  storage,     // The storage engine to use
  // whitelist: ['counter'] // Optional: only persist the counter slice
};

// 3. Create a new persisted reducer
const persistedReducer = persistReducer(persistConfig, rootReducer);

// 4. Configure the store with the persisted reducer
export const store = configureStore({
  reducer: persistedReducer,
  middleware: (getDefaultMiddleware) =>
    getDefaultMiddleware({
      serializableCheck: false, // Recommended to disable for redux-persist
    }),
});

// 5. Create the persistor
export const persistor = persistStore(store);
```

**Step 4: Provide the store and persistor to your app**

```tsx
// main.tsx
import React from 'react';
import ReactDOM from 'react-dom/client';
import { Provider } from 'react-redux';
import { PersistGate } from 'redux-persist/integration/react';
import { store, persistor } from './store';
import App from './App';

ReactDOM.createRoot(document.getElementById('root')!).render(
  <React.StrictMode>
    <Provider store={store}>
      {/* PersistGate delays rendering until the persisted state is retrieved */}
      <PersistGate loading={null} persistor={persistor}>
        <App />
      </PersistGate>
    </Provider>
  </React.StrictMode>
);
```

With this setup, any change to your counter slice will be automatically saved.

## 🧠 **Real-World Case Study: "The 30-Minute Setup"**

- **The Challenge:** A team building a content-heavy PWA needed a way for users to save their "reading progress" through long articles. They knew they needed a persistent state solution but were worried it would be complex to implement.
- **The Process:** Following the official docs, they installed RTK and `redux-persist`. They created a `progressSlice` that stored a map of `articleId` to `scrollPosition`. They then followed the exact configuration steps outlined above.
- **The Result:** In less than 30 minutes of work, they had a fully functioning, persistent state management system. A user could read halfway through an article, close the PWA, come back a day later, and the app would automatically scroll them back to their exact position. The developer experience was so smooth that it became their standard architecture for all future projects.

## 🤔 **Reflective Questions**

1. What is the purpose of the `<PersistGate>` component in the React application? What would happen if you forgot to include it?
2. The example uses `localStorage`. What are the advantages and disadvantages of `localStorage` compared to a more advanced storage engine like `IndexedDB`?
3. Why is it recommended to disable the `serializableCheck` middleware when using `redux-persist`?

## 📚 **Further Reading**

- **Redux Toolkit:** [Quick Start](https://redux-toolkit.js.org/introduction/quick-start) (The official tutorial is the best place to start).
- **Redux Persist:** [Documentation](https://github.com/rt2zz/redux-persist) (Official repository with configuration details).

## 📝 **Mini Task (Production)**

Get your hands dirty and build your first persistent store.

1. In a new or existing React project, install `@reduxjs/toolkit`, `react-redux`, and `redux-persist`.
2. Create the `counterSlice.ts` file exactly as shown in the example.
3. Create the `store.ts` file and configure the store with `persistReducer`.
4. Update your `main.tsx` (or `index.tsx`) to wrap your `<App>` with the `<Provider>` and `<PersistGate>`.
5. In your `App.tsx`, display the counter value and a button to increment it. Verify that when you increment the counter and then refresh the page, the value is remembered.