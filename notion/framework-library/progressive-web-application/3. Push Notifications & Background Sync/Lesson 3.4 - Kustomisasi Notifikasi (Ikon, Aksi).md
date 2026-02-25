# 💡 CT01-3.4: Beyond the Title: Customizing Notifications

**Outline:**

- **Making an Impact (Presentation):** Exploring the options for creating rich, interactive notifications that go beyond simple text.
- **The Options Bag (Practice):** A deep dive into the `options` parameter of `showNotification`, and handling the `notificationclick` event.
- **Building an Interactive Notification (Production):** Time to add an icon, a body, and an action button to your notification.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - Making an Impact**

A notification with just a title is functional, but it's not engaging. The Notifications API allows us to create a much richer experience, closer to what users expect from native apps. We can add icons, images, custom vibration patterns, and most importantly, **action buttons**.

Action buttons transform a notification from a simple alert into a mini-interactive UI. A user can perform common tasks (like "Reply", "Archive", or "Mark as Read") directly from the notification, without even having to open your PWA.

**The Core Philosophy:** A good notification provides context and offers relevant actions. By customizing the look and feel, you make the notification more recognizable as coming from your app. By adding actions, you save the user time and make your PWA feel more integrated with their operating system.

### **Part 2: Practice - The Options Bag**

The second argument to `self.registration.showNotification(title, options)` is a powerful options object. Here are some of the most important properties:

- `body`: A string of text to display below the title.
- `icon`: A URL to a small icon. Usually your app's logo.
- `badge`: A URL to a small, monochrome icon used in the OS status bar (Android only).
- `image`: A URL to a larger image to display within the notification content.
- `actions`: An array of objects, where each object defines an action button with a `title` (the button text) and an `action` (a string ID).

**Handling Clicks:** When the user clicks the notification body or an action button, a `notificationclick` event is fired in your service worker. The `event` object tells you which action was clicked.

```
// In your service worker file (e.g., src/sw.ts)

// ... push event handler ...

// Add a listener for notification clicks
self.addEventListener('notificationclick', (event) => {
  const notification = event.notification;
  const action = event.action; // The ID of the action button clicked

  console.log(`Notification clicked. Action: '${action}'`);

  // Close the notification
  notification.close();

  // Example of handling actions
  if (action === 'archive') {
    // TODO: Perform the archive action, maybe call an API
    console.log('Archiving item...');
  } else {
    // Default action (if body is clicked or no specific action)
    // Focus an existing window or open a new one
    event.waitUntil(
      clients.openWindow('<https://your-app-url.com>')
    );
  }
});

// Example of showing a notification with actions in the 'push' event
const options = {
  body: 'Your meeting starts in 5 minutes.',
  icon: '/icons/icon-192x192.png',
  actions: [
    { action: 'snooze', title: 'Snooze' },
    { action: 'archive', title: 'Archive' }
  ]
};
self.registration.showNotification('Calendar Reminder', options);
```

## 🧠 **Real-World Case Study: A Chat App (e.g., Signal, Discord)**

When you receive a message in a modern chat PWA, the notification is rich. It shows the sender's avatar (`icon`), the message content (`body`), and often an action button to "Reply". When you click "Reply", the app opens and automatically focuses the correct chat window. This is achieved by passing data in the notification (`data` property) and using that data in the `notificationclick` handler to open a specific URL like `clients.openWindow('/chats/123')`.

## 🤔 **Reflective Questions**

1. What is the difference between the `icon` and the `badge`? When would you see each one?
2. The `notificationclick` handler should always close the notification (`notification.close()`). Why is this good practice?
3. How can you pass a URL from your server to the `notificationclick` event handler, so that clicking the notification opens a specific page? (Hint: look at the `data` property on the `options` object).

## 📚 **Further Reading**

- **MDN - Notification API:** [The full list of available options](https://developer.mozilla.org/en-US/docs/Web/API/Notification)
- **Web.dev - Notification Actions:** [A guide to using action buttons](https://www.google.com/search?q=https://web.dev/push-notifications-notification-behaviour/%23actions)

## 📝 **Mini Task (Production)**

Let's build a rich, interactive notification.

**Task:**

1. **Server-side:** Modify your `/api/send-notification` endpoint. The payload should now be a more detailed JSON object:
    
    ```
    {
      "title": "Project Update",
      "body": "Task 'Deploy to Staging' was completed.",
      "icon": "/img/checkmark.png"
    }
    ```
    
    (You'll need to add an icon image to your `public` folder).
    
2. **Service Worker (`push` event):** Update your push handler to parse this new JSON structure and use the `title`, `body`, and `icon` properties in the `showNotification` options.
    
3. **Service Worker (`notificationclick` event):** Add a `notificationclick` event listener. For now, its only job should be to log the event to the console and then open a new window to your app's root URL using `clients.openWindow('/')`.
    
4. Rebuild, test, and send a notification. It should now appear with your custom content, and clicking it should open your PWA.