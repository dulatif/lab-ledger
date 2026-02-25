# 💡 CT01-2.4: The Graceful Exit: Handling Unsubscriptions

**Outline:**

- **The User's Right to Leave (Presentation):** Understanding the importance of providing a clean and easy way to unsubscribe.
- **Undoing the Subscription (Practice):** Using `getSubscription()` and `unsubscribe()` to remove a subscription, and notifying the server.
- **Building the Toggle (Production):** Time to create a complete UI that reflects the current subscription state and allows users to subscribe or unsubscribe.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The User's Right to Leave**

Just as we must ask for permission respectfully, we must also provide a way for the user to change their mind. Offering a clear and simple "unsubscribe" option within your app's UI is a critical part of building user trust.

The alternative is that the user, annoyed by notifications, goes into the browser settings and revokes permission. This is bad for two reasons:

1. It's a poor user experience for them.
2. Our application has no way of knowing they did this. Our server will continue sending push messages to an endpoint that is now invalid. Sending too many messages to invalid endpoints can get our server temporarily blocked by the Push Service.

**The Core Philosophy:** A user-initiated unsubscription is a two-step process. First, we must tell the Push Service to invalidate the subscription. Second, we must tell our own Application Server to delete that subscription from its database.

### **Part 2: Practice - Undoing the Subscription**

The `PushManager` interface provides the tools we need.

1. **`getSubscription()`**: This method returns a Promise that resolves with the current `PushSubscription` object if one exists, or `null` otherwise.
2. **`subscription.unsubscribe()`**: This method, called on a subscription object, tells the Push Service to invalidate it. It returns a Promise that resolves with a boolean indicating success.

Here's the full client-side workflow for unsubscribing:

```ts
async function unsubscribeUser() {
  const swRegistration = await navigator.serviceWorker.ready;
  try {
    // 1. Get the current subscription
    const subscription = await swRegistration.pushManager.getSubscription();

    if (subscription) {
      // 2. Tell the push service to unsubscribe
      const successful = await subscription.unsubscribe();
      if (successful) {
        console.log('User has been unsubscribed.');
        // 3. Tell our server to delete the subscription
        await sendUnsubscriptionToServer(subscription);
      }
    }
  } catch (error) {
    console.error('Error unsubscribing user: ', error);
  }
}

async function sendUnsubscriptionToServer(subscription) {
  // This looks very similar to our subscribe function,
  // but it calls a different endpoint.
  try {
    await fetch('/api/unsubscribe', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify({ endpoint: subscription.endpoint })
    });
    console.log('Unsubscription sent to server.');
  } catch (error) {
    console.error('Error sending unsubscription to server: ', error);
  }
}
```

Notice that for unsubscribing, we only need to send a unique identifier for the subscription (the endpoint is perfect for this) to our server so it can be deleted.

## 🧠 **Real-World Case Study: UI State Management**

A great PWA makes this seamless. It uses a state management solution (like React's `useState` or `useEffect`) to check the subscription status on load using `pushManager.getSubscription()`.

- If a subscription exists, it sets a state variable `isSubscribed` to `true`.
- The UI then renders an "Unsubscribe" button or a checked toggle.
- If the user clicks it, the app runs the `unsubscribeUser` logic and, on success, sets `isSubscribed` to `false`.
- The UI automatically re-renders to show the "Subscribe" button again. This provides instant, clear feedback to the user.

## 🤔 **Reflective Questions**

1. What should your UI do while the `unsubscribe()` promise is pending?
2. If the call to `subscription.unsubscribe()` succeeds but the `fetch` call to your server fails, what is the state of the system? Is this a major problem?
3. Is it possible for `pushManager.getSubscription()` to return a subscription that is no longer valid (e.g., the user revoked permission in settings)? How might you handle this?

## 📚 **Further Reading**

- **MDN - PushSubscription.unsubscribe():** [The official documentation for the unsubscribe method](https://developer.mozilla.org/en-US/docs/Web/API/PushSubscription/unsubscribe)
- **MDN - PushManager.getSubscription():** [Documentation for checking the current subscription](https://developer.mozilla.org/en-US/docs/Web/API/PushManager/getSubscription)

## 📝 **Mini Task (Production)**

Let's build the other half of the UI.

**Task:**

1. Implement the `unsubscribeUser` and `sendUnsubscriptionToServer` functions in your component.
2. Use React's `useState` and `useEffect` hooks to manage a boolean state variable, `isSubscribed`.
3. In the `useEffect` hook (that runs once on mount), call `pushManager.getSubscription()` and set the initial state of `isSubscribed` accordingly.
4. Render your UI conditionally. If `isSubscribed` is `true`, show an "Unsubscribe" button that calls `unsubscribeUser`. If `false`, show the "Subscribe" button that calls your `subscribeUser` logic.
5. Make sure to update the `isSubscribed` state after the subscribe/unsubscribe operations complete successfully to trigger a UI re-render. You now have a fully functional subscription management UI.