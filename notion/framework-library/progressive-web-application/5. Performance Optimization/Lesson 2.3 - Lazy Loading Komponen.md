# 💡 AM01.2.3: Deeper Cuts: Lazy Loading Components

**Outline:**

- **Splitting Below the Fold (Presentation):** Applying lazy loading to individual components for even finer-grained optimization.
- **Conditional Loading (Practice):** Using state and `Suspense` to load a component on demand.
- **Optimizing a Modal (Production):** Refactoring a complex, hidden component to be lazy-loaded.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - Splitting Below the Fold**

Route-based splitting is a massive win, but we can be even more precise. Think about a typical page. It might have a large, complex component that isn't immediately visible or necessary.

- A rich text editor inside a modal dialog.
- A heavy data visualization or charting library that's far down the page ("below the fold").
- A comments section that only appears after the user clicks "Show Comments."
- A/B testing components that only render for a subset of users.

Just like with routes, there's no reason to make the user pay the cost for these components during the initial page load. We can apply the exact same `React.lazy()` and `<Suspense>` pattern to **individual components** to defer their loading until they are actually needed. This technique is perfect for shaving off those last crucial kilobytes from your initial bundle and making the first interaction happen even faster.

### **Part 2: Practice - Conditional Loading**

Let's imagine we have a page with a button that opens a very complex `<SupportChatWindow>` component, which itself imports a large third-party library.

**Before (Bundled Together):**

```
// src/pages/ContactPage.tsx
import React, { useState } from 'react';
import SupportChatWindow from '../components/SupportChatWindow'; // <- This is heavy

function ContactPage() {
  const [isChatOpen, setIsChatOpen] = useState(false);

  return (
    <div>
      <h1>Contact Us</h1>
      <button onClick={() => setIsChatOpen(true)}>Open Support Chat</button>
      {isChatOpen && <SupportChatWindow />}
    </div>
  );
}
```

In this scenario, `SupportChatWindow` and all its dependencies are included in the initial JS chunk for `ContactPage`, even though 90% of users might never click that button.

**After (Component Lazy Loading):**

```
// src/pages/ContactPage.tsx
import React, { useState, lazy, Suspense } from 'react';

// 1. Lazy load the component
const LazySupportChat = lazy(() => import('../components/SupportChatWindow'));

function ContactPage() {
  const [isChatOpen, setIsChatOpen] = useState(false);

  return (
    <div>
      <h1>Contact Us</h1>
      <button onClick={() => setIsChatOpen(true)}>Open Support Chat</button>

      {/* 2. Conditionally render it inside a Suspense boundary */}
      {isChatOpen && (
        <Suspense fallback={<div>Loading chat...</div>}>
          <LazySupportChat />
        </Suspense>
      )}
    </div>
  );
}
```

Now, the code for the chat window will only be requested from the network when the user clicks the "Open Support Chat" button for the very first time. This is a huge performance improvement, especially for users who don't need this feature.

## 🧠 **Real-World Case Study: "The Heavy Charting Library"**

- **The Problem:** A business intelligence dashboard had its main filters and KPIs at the top of the page, but a series of very large, complex charts (using a 500KB charting library) at the bottom. The charts were "below the fold," but their code was in the main bundle, delaying the interactivity of the critical filters at the top.
- **The Fix:** The team identified the `<ChartBundle>` component that rendered all the charts. They wrapped its usage in a component that detects when it's about to scroll into the viewport (using the `IntersectionObserver` API). They then used `React.lazy()` to load `<ChartBundle>` only when it was about to become visible.
- **The Result:** The initial JS payload was cut in half. The TBT/INP for the page dropped from over 800ms to just 150ms. The filters at the top of the page became instantly responsive, dramatically improving the core user experience. The loading of the charts was now deferred, which was imperceptible to the user as it happened while they were scrolling down the page.

## 🤔 **Reflective Questions**

1. What are three good candidates for component-based lazy loading in a typical web application?
2. How does lazy loading components differ from lazy loading routes? When would you choose one over the other?
3. The `Suspense` fallback is crucial. What are some good "fallback" UIs you could show besides just "Loading..."? (Hint: think skeleton screens).

## 📚 **Further Reading**

- **React Docs:** [Suspense for Code Splitting](https://www.google.com/search?q=https://react.dev/reference/react/Suspense%23displaying-a-fallback-while-content-is-loading)
- **Web.dev:** [Lazy loading images and video](https://web.dev/articles/lazy-loading) (While this article is about assets, the _principle_ of deferring off-screen content is the same).

## 📝 **Mini Task (Production)**

Your app has a modal dialog that is only shown when a user clicks a button. This modal contains a complex form or component.

1. Identify the main component for your modal (e.g., `<EditProfileModal>`).
2. Refactor the page that opens the modal. Use `React.lazy()` to import the modal component.
3. Use component state (e.g., `isModalOpen`) to conditionally render your lazy-loaded modal.
4. Make sure to wrap the lazy-loaded component in a `<Suspense>` boundary so the user sees a loading indicator if the modal's code needs to be fetched.