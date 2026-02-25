# 💡 CM01-2.4: Closing the Loop: Notifying Users of Their Connection Status

**Outline:**

- **The Communication Channel (Presentation):** Understanding why explicit connection status feedback is crucial for user trust.
- **The Browser's Toolbox (Practice):** A hands-on guide to using `navigator.onLine` and the `online`/`offline` events in a React hook.
- **Building the Banner (Production):** Time to implement a real-time connection status indicator in your app.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Communication Channel**

We've designed our app to behave intelligently offline. But if we don't _tell_ the user what's happening, they can get confused. Why is that button disabled? Did my comment really send?

Providing clear, real-time feedback about the network status is the final piece of a trustworthy offline-first experience. It manages user expectations and explains why the UI might be in a different state.

A common pattern is a subtle, non-intrusive UI element:

- A "toast" notification that appears for a few seconds when the status changes.
- A persistent banner at the top or bottom of the screen.
- A small icon in the header (e.g., a cloud with a slash through it).

The message should be simple and clear: "You are currently offline. Some features may be limited." when the connection is lost, and "You are back online." when it's restored. This small bit of communication makes the entire experience feel intentional and robust, rather than broken.

### **Part 2: Practice - The Browser's Toolbox**

Modern browsers give us the tools we need to detect the connection state directly in our JavaScript.

1. **`navigator.onLine`**: This is a simple boolean property that returns `true` if the browser has a network connection and `false` if it doesn't. You can check it at any time.
2. **`online` and `offline` Events**: The `window` object emits two special events:
    - `'online'`: Fires when the connection is restored.
    - `'offline'`: Fires when the connection is lost.

We can combine these into a clean, reusable React hook to manage the state for us.

**Example: A `useOnlineStatus` Hook**

```
// hooks/useOnlineStatus.ts
import { useState, useEffect } from 'react';

export function useOnlineStatus() {
  const [isOnline, setIsOnline] = useState(navigator.onLine);

  useEffect(() => {
    function handleOnline() {
      setIsOnline(true);
    }

    function handleOffline() {
      setIsOnline(false);
    }

    window.addEventListener('online', handleOnline);
    window.addEventListener('offline', handleOffline);

    // Cleanup function to remove event listeners
    return () => {
      window.removeEventListener('online', handleOnline);
      window.removeEventListener('offline', handleOffline);
    };
  }, []); // Empty dependency array means this effect runs only once

  return isOnline;
}
```

Now, any component can easily use this hook to react to status changes:

```
// components/ConnectionBanner.jsx
import { useOnlineStatus } from '../hooks/useOnlineStatus';

function ConnectionBanner() {
  const isOnline = useOnlineStatus();

  if (isOnline) {
    return null; // Don't show anything if online
  }

  return (
    <div className="offline-banner">
      You are currently offline.
    </div>
  );
}
```

## 🧠 **Real-World Case Study: VS Code for the Web (vscode.dev)**

When you use VS Code in the browser, it works extensively offline. If your connection drops, a modal dialog appears that says "You are offline." It doesn't block you; you can close it and continue editing your files which are saved locally. When the connection returns, a small toast notification says "Successfully reconnected." This clear, explicit communication is essential for a professional tool where users cannot risk losing their work. It tells them exactly what the application's status is and what to expect.

## 🤔 **Reflective Questions**

1. The `navigator.onLine` property can sometimes be misleading. For example, you might be connected to a Wi-Fi router, but the router itself has no internet connection. How might this affect your app, and is there a more robust way to _truly_ check for connectivity? (Hint: think about what a service worker can do).
2. Where in your main application component tree would you place a component like `ConnectionBanner` to ensure it's always visible?
3. Besides a banner, what other UI elements could you change based on the return value of the `useOnlineStatus` hook?

## 📚 **Further Reading**

- **MDN Documentation:** [Navigator.onLine on MDN](https://developer.mozilla.org/en-US/docs/Web/API/Navigator/onLine)
- **A Complete Guide:** [A deep dive into creating an offline indicator with React](https://www.google.com/search?q=https://www.developerway.com/posts/how-to-detect-online-and-offline-status-in-react)

## 📝 **Mini Task (Production)**

Let's build that banner.

**Task:**

1. Create the `useOnlineStatus` hook as shown in the **Practice** section and add it to your project.
2. Create a new component (e.g., `StatusIndicator.jsx`).
3. Inside this new component, use the `useOnlineStatus` hook.
4. Have the component render a banner or a message only when the user is offline.
5. Add this `StatusIndicator` component to your main `App.jsx` layout so it's always present.
6. Test it! Run your app, open Chrome DevTools, go to the `Network` tab, and check the "Offline" checkbox to simulate losing the connection. Your banner should appear. Uncheck it, and the banner should disappear.