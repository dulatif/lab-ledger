# 💡 CT01-2.3: Phoning Home: Sending the Subscription to Your Server

**Outline:**

- **The Useless Subscription (Presentation):** Recognizing that a client-side subscription object needs a home on your server.
- **The `/subscribe` Endpoint (Practice):** A practical example of using `fetch` to POST the subscription object to a backend API.
- **Making the Connection (Production):** Time to implement the API call to send your subscription to a mock backend.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Useless Subscription**

We've successfully guided the user to grant permission and we've received a `PushSubscription` object from the Push Service. This object is our golden ticket. But right now, it only exists in the browser. If the user closes the tab, it's gone. For it to be useful, it must be persisted somewhere we can access it later: our application server.

The server needs to store this subscription so that when it's time to send a notification, it knows the exact endpoint to send the message to.

**The Core Philosophy:** The final step of the client-side subscription flow is to reliably transmit the `PushSubscription` object to your backend. This is a standard client-server API interaction. The client has the data; the server needs to save it. We must treat this like any other critical API call and handle both success and failure states.

### **Part 2: Practice - The `/subscribe` Endpoint**

Once we have the subscription object, we simply use the `fetch` API to send it to our server. The standard approach is to use a `POST` request to a dedicated endpoint, like `/api/subscribe`.

The body of the request will be the JSON representation of the subscription object. It's also crucial to send along authentication information (like a JWT in an `Authorization` header) so the server knows which user this subscription belongs to.

```ts
async function sendSubscriptionToServer(subscription) {
  try {
    const response = await fetch('/api/subscribe', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        // 'Authorization': `Bearer ${getAuthToken()}` // IMPORTANT: Include auth
      },
      body: JSON.stringify(subscription)
    });

    if (!response.ok) {
      throw new Error('Failed to send subscription to server.');
    }

    console.log('Subscription sent to server successfully.');

  } catch (error) {
    console.error('Error sending subscription to server: ', error);
  }
}

// --- Tying it all together ---
async function subscribeUser() {
  // ... get swRegistration and call pushManager.subscribe() ...
  const subscription = await swRegistration.pushManager.subscribe({ ... });

  // After getting the subscription, send it to the server
  await sendSubscriptionToServer(subscription);
}
```

On the server side (which we'll build in the next module), an Express handler for this might look like:

```ts
// Server-side example (Node.js/Express)
app.post('/api/subscribe', (req, res) => {
  const subscription = req.body;
  const userId = req.user.id; // From the auth middleware

  // TODO: Save the 'subscription' object to the database,
  // associated with the 'userId'.
  db.saveSubscription(userId, subscription);

  res.status(201).json({ message: 'Subscription saved.' });
});

```

## 🧠 **Real-World Case Study: Handling Network Failure**

What happens if the `fetch` call to `/api/subscribe` fails because the user's connection drops right after they grant permission? The user is subscribed to the push service, but our server doesn't know about it. A robust app would handle this. It could save the subscription to IndexedDB and try sending it again the next time the app loads. Or even better, it could use the Background Sync API (which we'll learn about later) to automatically re-attempt the `fetch` call once the connection is restored.

## 🤔 **Reflective Questions**

1. Why is a `POST` request more appropriate for this action than a `GET` or `PUT` request?
2. Besides the subscription object and an auth token, is there any other information you might want to send to the `/api/subscribe` endpoint? (Hint: think about device type, browser name, etc., for analytics).
3. How should your server handle a request to `/api/subscribe` from a user who is already subscribed (i.e., you already have a subscription with that endpoint in your DB)?

## 📚 **Further Reading**

- **MDN - Using Fetch:** [A comprehensive guide to the Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch)
- **REST API Design:** [Best practices for designing API endpoints](https://restfulapi.net/resource-naming/)

## 📝 **Mini Task (Production)**

Let's complete the client-side logic loop.

**Task:**

1. Create the `sendSubscriptionToServer` function as shown in the practice section.
2. Integrate this function into your `subscribeUser` flow. After the call to `pushManager.subscribe()` succeeds, it should immediately call `sendSubscriptionToServer` with the new subscription object.
3. For now, you don't have a real backend. Point the `fetch` call to a non-existent URL like `http://localhost:4000/api/subscribe`.
4. Run your code. The subscription should succeed, but the `fetch` call will fail and trigger your `catch` block. Verify this by checking the console logs. You have now correctly modeled the full client-side flow.