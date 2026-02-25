# 💡 CM01-2.3: Graceful Degradation: Designing for Offline Conditions

**Outline:**

- **Beyond the Blank Screen (Presentation):** Discussing UI/UX patterns to create a transparent and functional offline experience.
- **Visualizing Offline States (Practice):** UI mockups showing how to handle cached data, missing data, and pending actions.
- **Designing Your Offline UI (Production):** Time to plan the offline state for a feature in your own app.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - Beyond the Blank Screen**

So, we can load our app shell offline. That's a huge win. But what happens inside the shell? If we just show empty states or spinners that never resolve, the user is still stuck. A great offline UX is about more than just loading the UI; it's about designing for the reality of an absent network.

This means we need to think about several states:

1. **Read-Only Access:** The simplest and most crucial feature. The user should be able to view any content that has been previously cached. If they were reading an article, they should still be able to read it.
2. **Handling Missing Data:** What if the user navigates to a page for which there is no cached data? Instead of an error, show a clear message like, "This content will be available when you're back online."
3. **Disabling Actions:** Actions that absolutely require a network connection (like making a payment or live streaming) should be clearly disabled. A greyed-out button is much better than a button that does nothing or shows an error when clicked.
4. **Queueing Actions (Optimistic UI):** For actions like posting a comment, liking a photo, or sending a message, we can use an "optimistic" approach. The UI updates immediately as if the action was successful. The data is saved locally and placed in a queue. The app can then sync this queue with the server when the network returns. This feels incredibly seamless to the user.

The goal is transparency. The user should understand their current status and know what they can and cannot do.

### **Part 2: Practice - Visualizing Offline States**

Let's look at a messaging app UI.

- **State 1: Online**
    - The "Send" button is active. Messages have a "Delivered" or "Read" status.
- **State 2: Offline - Read-Only & Disabled Actions**
    - A banner at the top says "You are currently offline."
    - The user can still read the entire message history that was cached.
    - The "Start Video Call" button is greyed out and disabled.
- **State 3: Offline - Queued Actions (Optimistic UI)**
    - The user types a message and hits "Send."
    - The message immediately appears in the chat history, but with a small clock icon and a "Pending" status next to it.
    - The app has saved this message locally and will send it automatically when the connection is restored.

This approach keeps the app interactive and useful, building user confidence that their actions are not lost.

## 🧠 **Real-World Case Study: Trello**

Trello's PWA is a prime example of queuing actions. When you're offline, you can continue to organize your project. You can create new cards, edit descriptions, and drag-and-drop cards between lists. The UI updates instantly. Trello is simply recording all these "mutations" in a local queue. The moment your connection returns, you can see the network activity light up as it syncs all your offline changes with the server. It's a powerful and productive experience.

## 🤔 **Reflective Questions**

1. What are the risks of an "optimistic UI"? What happens if a queued action fails on the server when it tries to sync later (e.g., a comment is rejected for moderation)? How would you communicate that failure to the user?
2. Think about a data-heavy app like a stock trading platform. Which parts could be designed for read-only offline access, and which parts would need to be disabled?
3. How can "skeleton screens" (grey placeholder boxes) be used to improve the perceived performance of loading cached data?

## 📚 **Further Reading**

- **UX Planet:** [Designing Offline-First Experiences](https://www.google.com/search?q=https://uxplanet.org/designing-offline-first-experiences-5ad45a3b934)
- **Google Developers:** [The Offline Cookbook - On Update and Sync](https://www.google.com/search?q=https://web.dev/offline-cookbook/%23on-update-and-sync)

## 📝 **Mini Task (Production)**

Let's design a graceful offline experience for one feature.

**Task:** Choose one interactive feature from your application (e.g., submitting a form, adding an item to a list, posting a comment).

1. Describe or sketch the UI for this feature when the user is online.
2. Describe or sketch how the UI would change to handle being offline.
3. Will you disable the action, or will you use an optimistic UI and queue the action? Explain your choice.