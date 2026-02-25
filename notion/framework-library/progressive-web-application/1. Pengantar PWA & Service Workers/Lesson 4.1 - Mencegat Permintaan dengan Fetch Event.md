# 💡 CD04-4.1: Intercepting Requests with the Fetch Event [undone]

**Outline:**

- **The Modern Web App (Presentation):** Understanding the service worker's role as a network proxy.
- **The Engine Room (Practice):** Adding the `fetch` event listener to your service worker.
- **Your First Interception (Production):** Time to see your service worker monitor all network traffic.

## 📘 **Full Lesson Content**

### Part 1: Presentation - The Network Proxy

Everything we've done—registration, installation, activation—has been leading up to this. The `fetch` event is the heart of a service worker's power. It turns your service worker into a programmable network proxy that sits right between your web app and the network.

Every single network request made by your application, no matter how small, will trigger this event. This includes:

- Navigating to a new page (`/about`).
- Fetching the main JavaScript bundle (`app.123.js`).
- Loading a CSS file (`styles.456.css`).
- Requesting an image (`logo.png`).
- Making an API call (`/api/users`).

By listening for this event, you gain the ability to intercept the request and decide what to do with it. You can let it pass through to the network, block it, redirect it, or, most importantly, respond with something you've previously saved in your cache. This interception is what makes offline capabilities possible.

### Part 2: Practice - Listening for `fetch`

Adding the event listener is as simple as the `install` and `activate` events. We use `self.addEventListener('fetch', ...)`. The event object passed to our callback contains the `request` information.

For now, we won't change the behavior. We'll simply log the URL of every request that the service worker intercepts.