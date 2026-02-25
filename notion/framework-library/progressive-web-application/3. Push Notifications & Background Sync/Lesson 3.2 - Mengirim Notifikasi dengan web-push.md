# 💡 CT01-3.2: Pulling the Trigger: Sending Notifications with web-push

**Outline:**

- **The Push Protocol (Presentation):** A brief look at what the `web-push` library does for us under the hood (VAPID signing, encryption).
- **Configuring and Sending (Practice):** The practical steps of setting VAPID details and calling `webpush.sendNotification`.
- **Your First Push (Production):** Time to create a test endpoint that sends a "Hello World" notification to all subscribed clients.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Push Protocol**

We're ready to send a message. But we can't just send a plain JSON object to the push service endpoint. The Web Push Protocol requires the message to be properly encrypted and for the request to have headers containing our VAPID signature.

This is a complex process to implement manually. Thankfully, the `web-push` library handles all of this for us. It's a high-level API that abstracts away the low-level cryptography and protocol details.

**The Core Philosophy:** Our job is to provide `web-push` with three things:

1. Our VAPID keys for identification.
2. The `PushSubscription` object of the user we want to notify.
3. The payload (the data) we want to send.

The library then takes care of formatting the request, signing it, and sending it to the correct Push Service.

### **Part 2: Practice - Configuring and Sending**

Using the library is a two-step process in our `index.js` server file.

1. **Configuration:** First, we require the library and configure it with our VAPID keys. This only needs to be done once when the server starts. Your email is also required by some push services.
    
    ```ts
    // index.js
    const webpush = require('web-push');
    
    // ... other requires and setup ...
    
    const vapidPublicKey = 'YOUR_VAPID_PUBLIC_KEY';
    const vapidPrivateKey = 'YOUR_VAPID_PRIVATE_KEY';
    
    webpush.setVapidDetails(
      '<mailto:your-email@example.com>',
      vapidPublicKey,
      vapidPrivateKey
    );
    ```
    
2. **Sending:** To send a notification, we call `webpush.sendNotification()`. This function takes the subscription object and an optional payload string. It returns a promise.
    
    ```ts
    // Example of sending a notification inside an endpoint
    app.post('/api/send-notification', (req, res) => {
      const payload = JSON.stringify({ title: 'Hello from the Server!' });
    
      // Loop through all subscriptions and send a notification
      const sendPromises = subscriptions.map(sub =>
        webpush.sendNotification(sub, payload)
      );
    
      // Wait for all notifications to be sent
      Promise.all(sendPromises)
        .then(() => res.status(200).json({ message: 'Notifications sent.' }))
        .catch(err => {
          console.error("Error sending notification, reason: ", err);
          res.sendStatus(500);
        });
    });
    ```
    

## 🧠 **Real-World Case Study: Error Handling**

What happens if a subscription is expired or invalid? The `webpush.sendNotification` promise will reject, often with a status code like `410 Gone`. A robust server needs to handle this. In your `.catch()` block, you should inspect the error. If the status code is `410` or `404`, it means the subscription is permanently invalid, and you should delete it from your database to stop trying to send messages to it. This cleanup is essential for maintaining a healthy sender reputation with push services.

## 🤔 **Reflective Questions**

1. Why does `webpush.sendNotification` return a promise?
2. The payload we send is a string. If we want to send structured data (like a title, body, and icon), what format should that string be in?
3. In the example, we're sending the notification to _every_ subscriber. How would you change the logic to send a notification to only a single, specific user?

## 📚 **Further Reading**

- **`web-push` Library README:** [The full documentation with all options](https://www.google.com/search?q=https://www.npmjs.com/package/web-push)
- **Web Push Protocol Spec:** [For the curious, the low-level details of the protocol](https://datatracker.ietf.org/doc/html/rfc8030)

## 📝 **Mini Task (Production)**

Let's send our first real push notification.

**Task:**

1. In your `index.js` server, add your real VAPID public and private keys.
2. Configure `web-push` using `setVapidDetails`.
3. Create a new `POST` endpoint at `/api/send-notification`.
4. Inside this endpoint, define a simple payload string, like `JSON.stringify({ title: 'Test Notification' })`.
5. Loop through your `subscriptions` array and use `webpush.sendNotification` to send the payload to each one.
6. **Test it:**
    - Start your server.
    - Open your PWA and subscribe to notifications.
    - Use a tool like Postman, `curl`, or another browser tab to make a `POST` request to `http://localhost:4000/api/send-notification`.
    - You should receive a push notification on your device! (Note: It won't display anything yet, but the push event _will_ be received by the browser).