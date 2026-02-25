# 💡 AT01.3.3: The Network Pulse: Displaying Online-Offline Status in the UI

**Outline:**

- **The Unreliable Signal (Presentation):** Why the browser's default `navigator.onLine` isn't enough for a truly robust PWA.
- **The Two-Source Truth (Practice):** Combining browser events with service worker feedback for a reliable status.
- **Building the Status Indicator (Production):** Creating a React component that reflects the true network state.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Unreliable Signal**

A key feature of a native-feeling PWA is its ability to inform the user about their network status. Is the app online or offline? The browser gives us a simple property for this: `navigator.onLine`. It returns `true` or `false`.

The problem is, `navigator.onLine` can be a liar. It only tells you if the device is connected to a network (like Wi-Fi or cellular). It doesn't know if that network actually has a connection to the internet. You can be connected to a Wi-Fi router with no internet uplink, and `navigator.onLine` will happily report `true`. This is known as "Lie-Fi".

For a robust PWA, relying on this is not enough. We need a more reliable source of truth. The best source of truth is the service worker itself. Since it handles all outgoing `fetch` requests, it is the first part of our application to know when a request truly fails due to a network error. By combining the quick feedback from the browser's `online`/`offline` events with definitive feedback from our service worker, we can create a network status indicator that users can trust.

### **Part 2: Practice - The Two-Source Truth**

Here's how we combine the two sources into a global Redux state.

**Step 1: Create a Redux slice for network status**

```ts
// features/network/networkSlice.ts
import { createSlice, PayloadAction } from '@reduxjs/toolkit';

const networkSlice = createSlice({
  name: 'network',
  initialState: { isOnline: navigator.onLine },
  reducers: {
    setStatus: (state, action: PayloadAction<boolean>) => {
      state.isOnline = action.payload;
    },
  },
});

export const { setStatus } = networkSlice.actions;
export default networkSlice.reducer;
```

**Step 2: Listen to browser events in your app** This gives us instant feedback when the user connects or disconnects from a network.

```tsx
// App.tsx
import { useEffect } from 'react';
import { useDispatch } from 'react-redux';
import { setStatus } from './features/network/networkSlice';

function App() {
  const dispatch = useDispatch();

  useEffect(() => {
    const goOnline = () => dispatch(setStatus(true));
    const goOffline = () => dispatch(setStatus(false));

    window.addEventListener('online', goOnline);
    window.addEventListener('offline', goOffline);

    return () => {
      window.removeEventListener('online', goOnline);
      window.removeEventListener('offline', goOffline);
    };
  }, [dispatch]);

  // ... rest of your app
}
```

**Step 3 (The crucial part): Get feedback from the service worker** The service worker confirms if a request fails, giving us the definitive "offline" signal.

```ts
// sw.ts
// In your fetch event handler
self.addEventListener('fetch', (event) => {
  event.respondWith(
    fetch(event.request).catch(async (error) => {
      // A fetch failed, likely due to network error.
      // This is a strong signal that we are offline.
      const clients = await self.clients.matchAll();
      clients.forEach(client => {
        client.postMessage({ type: 'SET_OFFLINE' });
      });
      // Then, serve a fallback from cache or an offline page
      return caches.match('/offline.html');
    })
  );
});
```

Your app would then listen for the `'SET_OFFLINE'` message and dispatch `setStatus(false)`.

## **🧠 Real-World Case Study: "The False Hope"**

- **The Problem:** A PWA for field technicians used `navigator.onLine` to enable or disable a "Sync Data" button. In many industrial sites, technicians would be connected to a local Wi-Fi network that had no internet access. The browser reported `true` for `navigator.onLine`, so the "Sync Data" button was enabled. Technicians would click it, and the app would hang for 30 seconds before finally timing out with an error.
- **The Solution:** The team implemented the two-source pattern. They kept the browser events for quick UI updates but added logic to their service worker's fetch handler. If any API request failed with a network error, the service worker would `postMessage` to the UI, forcing the network state to `offline`.
- **The Result:** Now, when a technician entered a "Lie-Fi" zone, the first failed API call would immediately and correctly update the app's state to `offline`. The "Sync Data" button would become disabled, and a "You are currently offline" message would appear. This managed user expectations correctly and eliminated the frustrating "false hope" of the enabled button.

## 🤔 **Reflective Questions**

1. How could you make the "online" status even more reliable? (Hint: what if the service worker successfully fetches something after being offline?).
2. What is the `online` event on `window` good for, if not for a definitive status check? (Hint: it's a great trigger to _start_ a process, like re-trying a failed request queue).
3. How does this pattern of displaying network status improve the overall user experience and trust in your application?

## 📚 **Further Reading**

- **MDN:** [Navigator.onLine](https://developer.mozilla.org/en-US/docs/Web/API/Navigator/onLine)
- **Web.dev:** [Online and offline events](https://www.google.com/search?q=https://web.dev/articles/online-and-offline-events)

## 📝 **Mini Task (Production)**

Let's build the UI for your network status.

1. Create the `networkSlice` in your Redux store as shown in the example.
2. In your main `App.tsx` component, add the `useEffect` hook to listen for the global `online` and `offline` events and dispatch the `setStatus` action accordingly.
3. Create a new component called `<NetworkIndicator />`. This component should use `useSelector` to get the `isOnline` value from your `networkSlice`.
4. If `isOnline` is `false`, the component should render a small, noticeable banner or toast (e.g., a red bar at the top of the screen) that says "You are currently offline."
5. Place this `<NetworkIndicator />` component in your main app layout so it's always visible. Test it by using the "Offline" checkbox in Chrome DevTools.