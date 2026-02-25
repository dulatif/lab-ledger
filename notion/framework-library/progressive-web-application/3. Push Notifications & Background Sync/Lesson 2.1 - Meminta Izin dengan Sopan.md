# 💡 CT01-2.1: The Right Way to Ask: Permission UX

**Outline:**

- **Permission is a Privilege (Presentation):** Understanding why you must earn the user's trust before asking for notification permission.
- **The Permission API (Practice):** A hands-on look at the JavaScript API for checking and requesting permission.
- **Building the "Soft Prompt" (Production):** Time to create a user-friendly UI that explains the value of your notifications.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - Permission is a Privilege**

Team, let's establish rule number one for push notifications: **Never, ever ask for permission on page load.**

The default browser permission prompt is a modal, blocking, and often scary-looking dialog. . When a user lands on a site for the first time and is immediately hit with this, their instinct is to click "Block". This is a disaster for us, because if a user blocks notifications, it is very, very difficult to get them to change their mind later.

**The Core Philosophy:** We must treat notification permission as a transaction of trust. We earn that trust by first providing value and then explaining _why_ we need the permission. The best practice is a "soft prompt" or a two-click approach.

1. **First Click:** The user interacts with a UI element _you_ control (a button, a toggle). This element clearly explains the benefit, e.g., "Notify me about new messages".
2. **Second Click:** Only after the user shows intent by clicking your custom UI do we trigger the actual browser permission prompt.

This way, the user understands the context and is far more likely to grant permission.

### **Part 2: Practice - The Permission API**

The JavaScript for this is straightforward. The `Notification` object, available on the `window`, is our entry point.

The permission status can be in one of three states:

- `'granted'`: The user has already given permission. We're good to go.
- `'denied'`: The user has explicitly blocked notifications. We should not bother them again.
- `'default'`: The user has not yet made a choice. This is our opportunity to ask.

We can check the current status and request permission like this:

```ts
// Check the current permission status
if (Notification.permission === 'granted') {
  console.log('Permission already granted.');
  // We can proceed to subscribe the user
} else if (Notification.permission === 'denied') {
  console.log('Permission has been denied.');
  // We should respect the user's choice and do nothing
} else {
  // If status is 'default', we can ask
  askPermission();
}

async function askPermission() {
  // requestPermission() returns a Promise
  const permission = await Notification.requestPermission();
  if (permission === 'granted') {
    console.log('Permission was granted!');
  } else {
    console.log('Permission was not granted.');
  }
}
```

## **🧠 Real-World Case Study: X (formerly Twitter)**

When you use the X PWA, it doesn't ask for notification permission on your first visit. Instead, after you've used the app for a while, a small, non-intrusive banner might appear at the bottom of the screen saying something like "Never miss a thing. Get notifications for mentions and DMs." with an "Enable notifications" button. This is a perfect soft prompt. It's contextual, it explains the value, and it waits until the user is already engaged with the app.

## 🤔 **Reflective Questions**

1. What happens if a user simply closes or ignores the browser's permission prompt instead of clicking "Allow" or "Block"? What state does the permission remain in?
2. Why is it a bad idea to hide the rest of your UI behind a permission request?
3. Think about mobile vs. desktop. Is there a difference in how intrusive a permission prompt feels on a small screen versus a large one?

## 📚 **Further Reading**

- **MDN - Notifications API:** [The definitive guide to the Notifications API](https://developer.mozilla.org/en-US/docs/Web/API/Notifications_API/Using_the_Notifications_API)
- **Web.dev - Permission UX:** [Best practices for permission UX](https://www.google.com/search?q=https://web.dev/permissions-ux/)

## 📝 **Mini Task (Production)**

Let's build a simple permission-requesting component.

**Task:**

1. In your React app, create a new component called `NotificationButton`.
2. This component should render a single button with the text "Enable Notifications".
3. When the button is clicked, it should execute a function that:
    - Checks the current `Notification.permission` status.
    - If the status is `'granted'` or `'denied'`, it should simply `console.log` the status.
    - If the status is `'default'`, it should call `Notification.requestPermission()` and then log the result of that promise.
4. Add this button to your main app page and test its behavior in all three scenarios. (You can reset permission for a site by clicking the lock icon in the URL bar).