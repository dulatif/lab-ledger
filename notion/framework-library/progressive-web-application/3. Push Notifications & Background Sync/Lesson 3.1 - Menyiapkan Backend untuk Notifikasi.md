# 💡 CT01-3.1: The Command Center: Setting Up Your Backend

**Outline:**

- **The Server's Role (Presentation):** Understanding why a backend is non-negotiable for storing subscriptions and triggering pushes.
- **Building the Foundation (Practice):** A boilerplate Node.js and Express setup for handling subscription data.
- **Creating the Endpoints (Production):** Time to build the `/api/subscribe` and `/api/unsubscribe` endpoints that your client will call.

## 📘 **Full Lesson Content**

### **Part 1: Presentation - The Server's Role**

Team, we've mastered the client-side flow. The PWA can now ask for permission and get a subscription object. But that subscription is useless unless we have a central "command center" to store it and decide when to use it. This is the job of our **Application Server**.

The backend has two primary responsibilities in the push notification workflow:

1. **Subscription Management:** It needs to provide API endpoints for clients to send their new subscription objects for storage (e.g., in a database) and to request their removal when they unsubscribe.
2. **Push Initiation:** It contains the business logic that determines _when_ a notification should be sent. When triggered, it retrieves the relevant subscriptions from the database and sends the push message.

**The Core Philosophy:** The client is responsible for _obtaining_ the subscription; the server is responsible for _managing_ and _using_ it. For our course, we'll use a simple in-memory store, but in a real-world application, you would use a persistent database like PostgreSQL, MongoDB, or Firestore.

### **Part 2: Practice - Building the Foundation**

Let's set up a minimal backend using Node.js and Express. It's lightweight, fast, and perfect for this job.

1. **Initialize a new Node.js project:**
    
    ```bash
    mkdir notification-server
    cd notification-server
    npm init -y
    npm install express body-parser cors web-push
    ```
    
2. **Create a server file (e.g., `index.js`):**
    
    ```ts
    // index.js
    const express = require('express');
    const bodyParser = require('body-parser');
    const cors = require('cors');
    
    const app = express();
    const port = 4000;
    
    // Middleware
    app.use(cors()); // Allow requests from our frontend
    app.use(bodyParser.json());
    
    // In-memory "database" for subscriptions
    let subscriptions = [];
    
    app.get('/', (req, res) => {
      res.send('Notification Server is running!');
    });
    
    // TODO: Add /subscribe and /unsubscribe endpoints
    
    app.listen(port, () => {
      console.log(`Server listening at <http://localhost>:${port}`);
    });
    ```
    

This gives us a basic, running Express server. The `subscriptions` array will act as our temporary database.

## 🧠 **Real-World Case Study: Database Schema**

In a real application, you wouldn't just store the subscription object. Your database table or collection (e.g., `PushSubscriptions`) would likely have columns for:

- `id`: A unique primary key.
- `user_id`: A foreign key linking to your `Users` table. This is crucial for sending notifications to a specific user.
- `subscription_object`: A JSON or TEXT field to store the entire `PushSubscription` object.
- `device_info`: Optional text field to store user-agent, for analytics.
- `created_at`: A timestamp.

This allows you to query for all subscriptions belonging to a specific user, making targeted notifications possible.

## 🤔 **Reflective Questions**

1. Why is `cors` middleware necessary for this server?
2. What are the security risks of not associating a subscription with a `user_id` from an authenticated session?
3. How would you modify this simple server to handle multiple users, each with potentially multiple subscriptions?

## 📚 **Further Reading**

- **Express.js Docs:** [The official "Hello World" example for Express](https://expressjs.com/en/starter/hello-world.html)
- **Node.js `web-push` library:** [The library we'll use to send notifications](https://www.google.com/search?q=https://www.npmjs.com/package/web-push)

## 📝 **Mini Task (Production)**

Let's build the API endpoints your client-side code is already trying to call.

**Task:**

1. Inside your `index.js` file, add a `POST` endpoint for `/api/subscribe`.
2. The handler for this endpoint should take the subscription object from `req.body`, log it to the console, and add it to the `subscriptions` array.
3. Add a `POST` endpoint for `/api/unsubscribe`.
4. The handler should take the endpoint URL from `req.body`, find the matching subscription in the `subscriptions` array, and remove it.
5. Start your server (`node index.js`) and test your PWA's subscribe/unsubscribe functionality again. This time, you should see the logs appearing in your server's terminal, confirming the connection is working.