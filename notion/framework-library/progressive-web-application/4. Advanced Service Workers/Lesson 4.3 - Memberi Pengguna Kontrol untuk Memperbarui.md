# 💡 AD01.4.3: The "New Version" Button: Giving Users Control

**Outline:**

- **The UX of Updates (Presentation):** Why you should ask the user before reloading the application.
- **Building the UI (Practice):** Creating a React toast/notification component for the update prompt.
- **Conditional Rendering (Production):** Wiring up the new component to appear only when an update is available.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The UX of Updates**

We can now detect an update. The immediate temptation is to just call `location.reload()` inside the `onNeedRefresh` callback. **Don't do this.**

Forcefully reloading the page is a jarring and user-hostile experience. Imagine a user is in the middle of filling out a complex form or writing a long message. If the page suddenly reloads, their work is lost. They will be frustrated, and they will lose trust in your application.

The professional, user-centric approach is to _inform and ask_. We should:

1. **Inform:** Display a non-intrusive notification (often called a "toast" or "snackbar") that clearly states an update is available.
2. **Ask:** Provide a clear action, like an "Update" or "Refresh" button, that gives the user control. They can choose to update now, or they can ignore the notification and continue their task, updating later when it's convenient.

This pattern respects the user's workflow, maintains their state, and turns the update process from a potential disruption into a helpful, controlled feature.

### **Part 2: Practice - Building the UI**

Let's create a simple but effective `ReloadPrompt` component in React. It will be a fixed-position element that appears at the bottom of the screen.

```ts
// src/ReloadPrompt.tsx

import React from 'react';
import { useRegisterSW } from 'virtual:pwa-register/react';
import './ReloadPrompt.css'; // We'll add some simple styles

function ReloadPrompt() {
  // The 'useRegisterSW' hook provides the same functionality as registerSW,
  // but in a way that's easier to use within React components.
  const {
    offlineReady: [offlineReady, setOfflineReady],
    needRefresh: [needRefresh, setNeedRefresh],
    updateServiceWorker,
  } = useRegisterSW({
    onRegistered(r) {
      console.log('SW Registered:', r);
    },
    onRegisterError(error) {
      console.log('SW registration error:', error);
    },
  });

  const close = () => {
    setOfflineReady(false);
    setNeedRefresh(false);
  };

  // We only render the component if 'needRefresh' is true
  if (!needRefresh) {
    return null;
  }

  return (
    <div className="ReloadPrompt-container">
      <div className="ReloadPrompt-message">
        A new version is available, click on reload button to update.
      </div>
      <button className="ReloadPrompt-button" onClick={() => updateServiceWorker(true)}>
        Reload
      </button>
      <button className="ReloadPrompt-button" onClick={() => close()}>
        Close
      </button>
    </div>
  );
}

export default ReloadPrompt;
```

And the CSS:

```css
/* src/ReloadPrompt.css */
.ReloadPrompt-container {
  position: fixed;
  bottom: 1rem;
  right: 1rem;
  padding: 1rem;
  border: 1px solid #888;
  border-radius: 0.5rem;
  background-color: white;
  box-shadow: 0 4px 6px rgba(0,0,0,0.1);
  display: flex;
  align-items: center;
  gap: 1rem;
  z-index: 1000;
}
.ReloadPrompt-message {
  color: #333;
}
.ReloadPrompt-button {
  padding: 0.5rem 1rem;
  border: 1px solid #333;
  border-radius: 0.25rem;
  background-color: #f0f0f0;
  cursor: pointer;
}
```

This component uses a hook, `useRegisterSW`, which is a convenient React wrapper around the logic we wrote in the last lesson. It gives us a boolean state variable `needRefresh`. We render the UI only when this flag is `true`.

### **Part 3: Production - Conditional Rendering**

Now, we just need to integrate this component into our main application.

1. Create the two files: `src/ReloadPrompt.tsx` and `src/ReloadPrompt.css`.
2. Copy the code from the "Practice" section into them.
3. Go to your main `App.tsx` file and render the new component.
4. Remove the `registerSW` call from your `main.tsx`, as the `useRegisterSW` hook now handles it.

```tsx
// src/App.tsx
import React from 'react';
import ReloadPrompt from './ReloadPrompt'; // 1. Import the component

function App() {
  return (
    <div className="App">
      <header className="App-header">
        <h1>My Awesome PWA</h1>
      </header>
      <main>
        {/* Your app content goes here */}
      </main>
      <ReloadPrompt /> {/* 2. Render it here */}
    </div>
  );
}

export default App;
```

That's it! Now, when you run the full update flow (build, change SW, rebuild, refresh), you should see your beautifully styled notification appear at the bottom of the screen. The "Reload" button will already work because the `updateServiceWorker` function provided by the hook handles everything for us.

## 🧠 **Real-World Case Study: "The VS Code Update Notification"**

- **The Experience:** Anyone who uses Visual Studio Code is familiar with its update mechanism. A small blue dot appears on the settings cog icon. There's no pop-up, no forced reload. It's a subtle, passive notification. When the user is ready, they can click the icon and choose to restart and apply the update.
- **The Principle:** This is a masterclass in user-centric update design. It informs the user without interrupting them. It puts them in complete control. While our toast notification is a bit more direct, it follows the same core principle: **inform, don't demand**. This builds user trust and makes the application feel like a reliable tool, not an unpredictable website.

## 🤔 **Reflective Questions**

1. Why is a toast/snackbar a better UI pattern for this notification than a full-screen modal dialog?
2. In our component, we have a "Close" button. What is the user experience if they click "Close" and then continue to navigate around the app? Will they see the notification again?
3. The `useRegisterSW` hook simplifies things a lot. What do you think is happening inside that hook to manage the `needRefresh` state?

## 📚 **Further Reading**

- **Tooling:** [Vite PWA Plugin - React Hooks](https://www.google.com/search?q=https://vite-pwa-org.netlify.app/frameworks/react.html%23prompt-for-update) (Official docs for `useRegisterSW`).
- **UX Patterns:** [Designing for Interaction](https://www.google.com/search?q=https://www.nngroup.com/articles/toast-messages/) (Nielsen Norman Group on the use of Toasts/Snackbars).
- **React Docs:** [Conditional Rendering](https://react.dev/learn/conditional-rendering) (The core React concept that makes this UI possible).

## 📝 **Mini Task (Production)**

Fully implement the update prompt UI in your application.

1. Create the `ReloadPrompt.tsx` and `ReloadPrompt.css` files.
2. Add the provided code for the component and its styles.
3. Integrate the `<ReloadPrompt />` component into your main `App.tsx`.
4. Clean up your `main.tsx` file by removing the manual `registerSW` call.
5. Run the full update test cycle and verify that:
    - The prompt appears when an update is available.
    - Clicking "Reload" successfully updates the service worker and reloads the page.
    - Clicking "Close" dismisses the prompt.